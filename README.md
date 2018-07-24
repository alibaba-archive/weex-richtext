# weex-richtext
Weex richtext component for android/iOS

## Summary
Richtext is used to achieve inline <span> or `<a>` combined with inline-block `<image>`.It also support nested `<span>`, `<image>` and `<a>` with style inheritance and override. Thus, richtext can be considered as a more general `<text>` with more powerful usage. Generally, it can replace text with a little overhead unless you have a very long text which is updated frequently.

## Syntx
Only `<span>`, `<a>` and `<image>` are valid children of `<richtext>`. `<span>` and `<a>` are considered as display:inline, while `<image>` is considered as display:inline-block, which are default values and cannot be changed.

Children of `<richtext>` can be classified as two types of node, internal and external.

 - Internal node can have children nodes.
 - External node is not allowed to have any children.
 - `<span>` and `<a>` are internal node, while `<image>` is external node.

Richtext can be seen as a tree, and the max height of the tree is 255. Any styles on a node of which the height is greater than 255 has no effect.
  
## Note
 - `<a>` in iOS will have a color: blue style, and children of `<a>` can not override this style. This only happens for iOS, there is no default style and override restriction for `<a>` in Android.
 - `<image>` in richtext must have width and height style.
 - The content area of `<image>` will remain blank until the corresponding image is downloaded.
 - There is no nested richtext support for now.

## Attributes
Only the following attributes are supported in richtext.

### image
 - src
 - pseudo-ref (string), a ref assigned by developers and passed to the callback function of onitemclick
### a
 - href
### span
`<span>` doesn’t support any attributes, text must be set as the content of `<span>`, e.g. `<span>`Hello World`</span>`.

## Styles
Only a limited css style is supported in richtext. Any style not listed below is not supported.

 - `<span>`, `<a>` and `<richtext>` itself
   -  styles can be inherited: ```color, font-family, font-size, font-style, font-weight, line-height```
   
   - styles cannot be inherited: ```background-color```
 
  - `<span>` also supports the following style
    - style cannot be inherited: text-decoration: none | underline | line-through , the default value is none

 - `<richtext>` also supports the following style
   - style cannot be inherited: lines the max line of richtext. Must be a positive integer.

 - `<image>`
   - styles cannot be inherite: ```width, height```
  
## Events
### Richtext
#### onitemclick
This event will be fired when following condition met:

 - An `img` in richtext is click
 - None of the parent nor ancestor of the img is `a` tag

If the second condition doesn’t meet, weex will try to open the hyperlink of `a` tag instead.

**pseudo-ref** of img will be passed to the callback function of onitemclick when above condition met.

#### Common event
Weex common events are supported by the richtext node itself.

### Children of richtext
**None** of the children of richtext support event.
  
  
## Examples
```
  <template>
    <div>
        <richtext onitemclick="{{listener}}" style="color:red;text-overflow:ellipsis">
            <span>link</span>
            <a href="...">
                <image style="width:150; height:150" src="..." pseudo-ref="22"></image>
                <span style="font-size:42;color:#FF5400;">TAOBAO</span>
            </a>
            <image style="width:300; height:300" src="..." pseudo-ref="23"></image>
            <span>Transition Transition Transition Transition Transition Transition</span>
        </richtext>
    </div>
</template>

<script>
    module.exports = {
        methods: {
            listener: function (foo) {
                var modal = require('@weex-module/modal');
                modal.toast({
                'message': 'My pseudoRef is'+foo.pseudoRef,
                'duration': 3
                });
            }
        }
    }
</script>


<style>
.logo{width: 50;height: 50;}
.title{font-size:42; color: #FF5400;}
</style>
```

