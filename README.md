# Cocos2dx-ShatterSprite
Based on wantnon2/EffectNodes-for-cocos2dx


## Usage
    ShatterSprite* shatter_sprite = ShatterSprite::createWithTexture(a_Texture2D_instance));
    running_scene->addChild(shatter_sprite);
    shatter_sprite->run_shatter_action(0.5f);

### Example

    //build a sprite so we can reuse its texture
    cocos2d::Sprite* tex_sprite = draw_node_to_texture(some_cocos2d_Node);
    ShatterSprite* shatter_sprite = ShatterSprite::createWithTexture(tex_sprite->getTexture());
    
    //Position it in the corner for simplicity, and make sure it's visible    
    shatter_sprite->setAnchorPoint(cocos2d::Vec2::ZERO);
    shatter_sprite->setPosition({0, 0});
    running_scene->addChild(shatter_sprite, 999999999);

    //This sets the sprite to opacity 0, runs the ShatterAction, then removes the shattersprite
    shatter_sprite->run_shatter_action(0.5f);


### Helper function for writing Nodes to a RenderTexture

    cocos2d::Sprite* draw_node_to_texture(cocos2d::Node* node)
    {
        float width = 0, height = 0;
        width = node->getContentSize().width*node->getScaleX();
        height = node->getContentSize().height*node->getScaleY();

        cocos2d::Vec2 original_anchor = node->getAnchorPoint();
        cocos2d::Vec2 original_pos = node->getPosition();
        node->setAnchorPoint({0, 0});
        node->setPosition({0, 0});

        cocos2d::Director::getInstance()->getRenderer()->render();
        cocos2d::RenderTexture* rt = cocos2d::RenderTexture::create((int)width, (int)height);
        rt->begin();
        node->visit();
        rt->end();
        cocos2d::Director::getInstance()->getRenderer()->render();

        node->setAnchorPoint(original_anchor);
        node->setPosition(original_pos);

        cocos2d::Sprite* tex_sprite = dynamic_cast<cocos2d::Sprite*>(rt->getSprite());
        tex_sprite->removeFromParent();
        return tex_sprite;
    };
