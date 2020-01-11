## \*\*\*\*_The upstream repo is no longer being maintained_\*\*\*\*
_Vermilion Design has shifted the way that we develop for Gutenberg which means that this repo will no longer be relevant for our workflow. Additionally, there is a fundamental flaw with getting posts the way that this component does it. Data will not change when the source posts are updated. In order to circumvent this, you should use Server Side Rendering views, which are much easier to write and maintain. However, if you would like to still use this component, you should check out one of the forks._

# Gutenberg PostSelector

REQUIRES WordPress 5.0+

This is a React component built for Gutenberg that allows you to attach pages and posts like AddBySearch in the WP 5.0+ editor. 


## Installation
cd to your custom block plugin directory.

`npm install @vermilion/post-selector`

## Usage

block.js
```javascript
/**
 * BLOCK: Block Name
 */
//  Import CSS.
import './style.scss';

import PostSelector from '@vermilion/post-selector';

const { registerBlockType } = wp.blocks;
const { Fragment, RawHTML } = wp.element;
const { InspectorControls } = wp.editor;
const { PanelBody } = wp.components;

registerBlockType('vermilion/post-selector', {
  title: 'Post Selector',
  category: 'widgets',
  keywords: [''],
  attributes: {
    posts: {
      type: 'array',
      default: []
    },
  },

  edit({ attributes, setAttributes }) {
    return (
      <Fragment>
        <InspectorControls>
          <PanelBody title="Post Selector">
          
            <PostSelector
              onPostSelect={post => {
                const updatedPosts = [...attributes.posts, post];
                setAttributes({ posts: updatedPosts });
              }}
              posts={attributes.posts}
              onChange={newValue => {
                setAttributes({ posts: [...newValue] });
              }}
              postType={['post', 'page', 'gallery']}
              limit="3"
            />

          </PanelBody>
        </InspectorControls>
        <div>
          {attributes.posts.map(post => (
            <div>
              #{post.id}
              <h2>{post.title}</h2>
              <RawHTML>{post.excerpt}</RawHTML>
            </div>
          ))}
        </div>
      </Fragment>
    );
  },

  save({ attributes }) {
    return(
      <div>
        {attributes.posts.map(post => (
          <div>
            #{post.id}
            <h2>{post.title}</h2>
            <RawHTML>{post.excerpt}</RawHTML>
          </div>
        ))}
      </div>
    )
  }
});

```


### Props

`posts : <Post>[]`

posts should refer to an attribute in your block that is of type: 'array'. this is used internally by the component to update, re-order, and control deletion of posts from the selction.

`postType : <String> (optional)`

pass the singular name of a custom or built-in post type to limit results to that type (optional). 

`data <String>[] (optional)`

the data prop allows you to define an array of strings that map to object keys from the REST API. (does not support nesting right now).


`onPostSelect : function => <Post>[]`


onPostSelect runs when a user selects a new post from the suggestion list upon typing. It returns a new array of all selected posts and should replace the data in your posts attribute.

`onChange: function => <Post>[]`

onChange runs when the user reorders the array of posts or removes a post from the array. it returns a new array that should replace your posts attribute.

`limit: <Number> (optional)`

Limit the number of posts a user is allowed to select.
