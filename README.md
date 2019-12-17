# JSX Plus

[中文文档](./README.zh_CN.md)

Rax supports a JSX Extension Syntax by default-JSX+, which can help developers write JSX more quickly.

JSX+ is not a new concept. It is an extended instruction concept based on JSX.

## Why Do You Need JSX+

Rax uses JSX as the standard DSL to build its own ecology, but JSX also has certain limitations, so it has JSX+:

- Although JSX has flexible syntax, a large number of curly brackets JSX+ syntax have caused context switching and code readability to decline. JSX+'s instructions solve this problem well.

- JSX is essentially a JS expression, and the real DOM structure can be calculated at the runtime stage. JSX+ introduces some static template features to meet compilation optimization.

- Not creating new entities, instructions are widely accepted concepts in the community, more friendly to developers, and simpler expression of grammar sugar.

- Unify a set of JSX + grammar specifications similar to concepts to reduce existing and potential duplication.

## How To Use JSX+ in Rax

We have built-in JSX+ support in the Rax project builder, so you can use the following syntax directly without having to care about how to introduce it.

Of course, if you need to introduce it in a custom project(Including React/Preact/Vue.js and any VDOM based libraries are supported.), we provide [Rax official DEMO](https://github.com/jsx-plus/jsxplus-example-rax) And [Corresponding Babel plugin](https://github.com/jsx-plus) You can click on the corresponding link to view the details.

The following is a list of existing instructions in the JSX + 1.0 specification:



### 1. Conditional Judgment

Grammar:

```html
<View x-if={condition}>Hello</View>
<View x-elseif={anotherCondition}></View>
<View x-else>NothingElse</View>
```

Note: `x-elseif` It can appear multiple times, but the order must be x-if-> x-elseif-> x-else, and these nodes are sibling node relationships. If the order is wrong, instructions will be ignored.



### 2. Looping List

Grammar:

```html
{/* Array or Plain Object*/}
<tag x-for={item in foo}>{item}</tag>
  
<tag x-for={(item, key) in foo}>{key}: {item}</tag>
```

Instructions

1. If the loop object is an array, the key represents the loop index, and its type is Number.

1. When `x-for` And `x-if` When acting on the same node at the same time, the loop priority is greater than the condition, that is, the loop `item` and `index` can be used in sub-condition judgment.



### 3. Single Rendering

Trigger only when first rendering `createElement` and refer to the cache, re-render directly reuse the cache, used to improve the rendering efficiency and Diff performance without binding nodes.

Grammar:

```html
<p x-memo>this paragragh {mesasge} content will not change.</p>
```



### 4. Slot Instructions

A slot concept similar to WebComponents and provides slot scope.

Grammar:

```html
<tag x-slot:slotName="slotScope" />
```

Example:

```html
// Example
<Waterfall>
  <view x-slot:header>header</view>
  <view x-slot:item="props">{props.index}: {props.item}</view>
  <view x-slot:footer>footer</view>
</Waterfall>
<slot name="header" /> // slot
```

Compared with the traditional JSX:

```html
<Waterfall
  renderHeader={() => (<view>header</view>)}
  renderFooter={() => (<view>footer</view>)}
  renderItem={(item, index) => (<view>{index}: {item}</view>}
/>
```

Contrast applet:

```html
<Waterfall>
  <view slot="header">header</view>
  <view slot="item" slot-scope="props">{props.index}: {props.item}</view>
  <view slot="footer">footer</view>
</Waterfall>
```



### 5. Fragment Component

Provides empty components, does not generate UI, provides binding `x-if` `x-for` `x-slot` instruction.

Use:

```
<Fragment />
```



### 6. Class Name Binding

Grammar:

```html
<div x-class={{ item: true, active: val }} />
```

Reference implementation:

```html
<div className={classnames({ item: true, active: val})} />
```

`classnames` Method capability reference [npm package with the same name](https://npmjs.com/classnames).