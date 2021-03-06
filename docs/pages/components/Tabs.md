# Tabs

> Add quick, dynamic tab functionality to transition through panes of local content, even via dropdown menus. **Nested tabs are not supported**.

## Example

```html
<tabs>
  <tab title="Home">
    <p>Raw denim you probably haven't heard of them jean shorts Austin. Nesciunt tofu stumptown aliqua, retro synth master cleanse. Mustache cliche tempor, williamsburg carles vegan helvetica. Reprehenderit butcher retro keffiyeh dreamcatcher synth. Cosby sweater eu banh mi, qui irure terry richardson ex squid. Aliquip placeat salvia cillum iphone. Seitan aliquip quis cardigan american apparel, butcher voluptate nisi qui.</p>
  </tab>
  <tab title="Profile">
    <p>Food truck fixie locavore, accusamus mcsweeney's marfa nulla single-origin coffee squid. Exercitation +1 labore velit, blog sartorial PBR leggings next level wes anderson artisan four loko farm-to-table craft beer twee. Qui photo booth letterpress, commodo enim craft beer mlkshk aliquip jean shorts ullamco ad vinyl cillum PBR. Homo nostrud organic, assumenda labore aesthetic magna delectus mollit. Keytar helvetica VHS salvia yr, vero magna velit sapiente labore stumptown. Vegan fanny pack odio cillum wes anderson 8-bit, sustainable jean shorts beard ut DIY ethical culpa terry richardson biodiesel. Art party scenester stumptown, tumblr butcher vero sint qui sapiente accusamus tattooed echo park.</p>
  </tab>
  <tab title="@vue" group="Dropdown">
    <p>Etsy mixtape wayfarers, ethical wes anderson tofu before they sold out mcsweeney's organic lomo retro fanny pack lo-fi farm-to-table readymade. Messenger bag gentrify pitchfork tattooed craft beer, iphone skateboard locavore carles etsy salvia banksy hoodie helvetica. DIY synth PBR banksy irony. Leggings gentrify squid 8-bit cred pitchfork. Williamsburg banh mi whatever gluten-free, carles pitchfork biodiesel fixie etsy retro mlkshk vice blog. Scenester cred you probably haven't heard of them, vinyl craft beer blog stumptown. Pitchfork sustainable tofu synth chambray yr.</p>
  </tab>
  <tab title="@bootstrap" group="Dropdown">
    <p>Trust fund seitan letterpress, keytar raw denim keffiyeh etsy art party before they sold out master cleanse gluten-free squid scenester freegan cosby sweater. Fanny pack portland seitan DIY, art party locavore wolf cliche high life echo park Austin. Cred vinyl keffiyeh DIY salvia PBR, banh mi before they sold out farm-to-table VHS viral locavore cosby sweater. Lomo wolf viral, mustache readymade thundercats keffiyeh craft beer marfa ethical. Wolf salvia freegan, sartorial keffiyeh echo park vegan.</p>
  </tab>
</tabs>
<!-- tabs-example.vue -->
```

## Disable Tabs

Add `disabled` to `<tab>` to disable it.

```html
<tabs>
  <tab title="Home">
    <p>Home tab.</p>
  </tab>
  <tab title="Profile" disabled>
    <p>Profile tab.</p>
  </tab>
  <tab title="@vue" group="Dropdown">
    <p>@vue tab.</p>
  </tab>
  <tab title="@bootstrap" group="Dropdown" disabled>
    <p>@bootstrap tab.</p>
  </tab>
</tabs>
<!-- tabs-disable.vue -->
```

## Pull Right

Add `pull-right` to `<tab>` to pull it right. A grouped tab will be pulled to right if one of its sub-tabs has this prop set to `true`.

```html
<tabs>
  <tab title="Home">
    <p>Home tab.</p>
  </tab>
  <tab title="Profile">
    <p>Profile tab.</p>
  </tab>
  <tab title="@vue" group="Dropdown" pull-right>
    <p>@vue tab.</p>
  </tab>
  <tab title="@bootstrap" group="Dropdown">
    <p>@bootstrap tab.</p>
  </tab>
</tabs>
<!-- tabs-pull-right.vue -->
```

## Justified Style

Add `justified` to `<tabs>` to use justified style.

```html
<tabs justified>
  <tab title="Home">
    <p>Home tab.</p>
  </tab>
  <tab title="Profile">
    <p>Profile tab.</p>
  </tab>
  <tab title="Others">
    <p>Others tab.</p>
  </tab>
</tabs>
<!-- tabs-justified-style.vue -->
```

## With Callbacks

```html
<template>
  <tabs @change="onChange">
    <tab title="Home">
      <p>Home tab.</p>
    </tab>
    <tab title="Profile">
      <p>Profile tab.</p>
    </tab>
    <tab title="<i class='glyphicon glyphicon-bell'></i> Alert!" :html-title="true">
      <p>This tab has HTML title and callback function!</p>
    </tab>
  </tabs>
</template>
<script>
  export default {
    methods: {
      onChange (index) {
        if (index === 2) {
          window.alert('You clicked on a tab that has callback function!')
        }
      }
    }
  }
</script>
<!-- tabs-with-callbacks.vue -->
```

## With `nav-right` Slot

```html
<tabs>
  <tab title="Home">
    <p>Home tab.</p>
  </tab>
  <tab title="Profile">
    <p>Profile tab.</p>
  </tab>
  <form slot="nav-right">
    <select class="form-control" style="display: inline-block;width: auto">
      <option>option1</option>
      <option>option2</option>
      <option>option3</option>
      <option>option4</option>
    </select>
    <button type="button" class="btn btn-success">Button</button>
  </form>
</tabs>
<!-- tabs-with-nav-right-slot.vue -->
```

# API Reference

## [Tabs.vue](https://github.com/wxsms/uiv/tree/master/src/components/tabs/Tabs.vue)

### Props

Name                  | Type       | Default  | Required | Description
----------------      | ---------- | -------- | -------- | -----------------------
`v-model`             | Number     |          |          | The current tab index, use this to manual change tab index.
`justified`           | Boolean    | false    |          | Use justified style.
`transition-duration` | Number     | 150      |          | The tabs show / hide transition time in ms. Use 0 to disable transitions.

### Slots

Name        | Description
---------   | -----------------------
`default`   | The tabs content.
`nav-right` | The snip at right side of tab nav. Note: it won't display if using justified style.

### Events

Name        | Params | Description
----------- | ------ | ---------------
`change`    | index  | Fire after active tab changed, with the active index.

## [Tab.vue](https://github.com/wxsms/uiv/tree/master/src/components/tabs/Tab.vue)

### Props

Name             | Type       | Default  | Required | Description
---------------- | ---------- | -------- | -------- | -----------------------
`title`          | String     |          |          | The tab title.
`html-title`     | Boolean    | false    |          | Use HTML title.
`disabled`       | Boolean    | false    |          | Disable the tab.
`group`          | String     |          |          | Tabs nav with same group will in a dropdown list.
`pull-right`     | Boolean    | false    |          | Add `pull-right` class to the tab nav. A grouped tab will be pull to right if one of its sub-tabs has this prop set to `true`.
