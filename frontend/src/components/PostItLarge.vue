<template>
  <li
    @click="$emit('click')"
    :class="postit.status"
    class="post-it-large p-3 mr-4 mb-4"
    v-tooltip.top-center="text_ttipe"
  >
    <h3 class="">{{ title }}</h3>
    <p class="post-it-description p-0">{{ description }}</p>
  </li>

</template>

<script>

function textEllipsis(text, length){
  // Cuts text and appends ellipsis if text.length is greater than length.

  if (text.length < length)
    return text;
  return text.slice(0, length) + '...';
}


export default {
  name: "PostItLarge",
  props: ["postit"],
  computed: {
    title(){
      return textEllipsis(this.postit.title, 50);
    },
    description(){
      return textEllipsis(this.postit.description, 140);
    },
    text_ttipe(){
      return this.postit.title.length > 50 ? this.postit.title : ""
    }
  }
};
</script>

<style>
/* Post-it colors where defined in PostItSmall.vue */

.post-it-large{
  cursor: pointer;
  max-width: 20rem;
  min-width: 15rem;
  min-height: 15rem;
  box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.4);
  border-radius: 3px;
  list-style: none;
  overflow-wrap: break-word;
}

.post-it-description{
  font-size: 1.2rem;
  overflow-wrap: break-word;
}

/* Tooltip */
.tooltip {
  display: block !important;
  z-index: 10000;
}
 
.tooltip .tooltip-inner {
  background: black;
  color: white;
  border-radius: 16px;
  padding: 5px 10px 4px;
}
 
.tooltip .tooltip-arrow {
  width: 0;
  height: 0;
  border-style: solid;
  position: absolute;
  margin: 5px;
  border-color: black;
  z-index: 1;
}
 
.tooltip[x-placement^="top"] {
  margin-bottom: 5px;
}
 
.tooltip[x-placement^="top"] .tooltip-arrow {
  border-width: 5px 5px 0 5px;
  border-left-color: transparent !important;
  border-right-color: transparent !important;
  border-bottom-color: transparent !important;
  bottom: -5px;
  left: calc(50% - 5px);
  margin-top: 0;
  margin-bottom: 0;
}
 
.tooltip[x-placement^="bottom"] {
  margin-top: 5px;
}
 
.tooltip[x-placement^="bottom"] .tooltip-arrow {
  border-width: 0 5px 5px 5px;
  border-left-color: transparent !important;
  border-right-color: transparent !important;
  border-top-color: transparent !important;
  top: -5px;
  left: calc(50% - 5px);
  margin-top: 0;
  margin-bottom: 0;
}
 
.tooltip[x-placement^="right"] {
  margin-left: 5px;
}
 
.tooltip[x-placement^="right"] .tooltip-arrow {
  border-width: 5px 5px 5px 0;
  border-left-color: transparent !important;
  border-top-color: transparent !important;
  border-bottom-color: transparent !important;
  left: -5px;
  top: calc(50% - 5px);
  margin-left: 0;
  margin-right: 0;
}
 
.tooltip[x-placement^="left"] {
  margin-right: 5px;
}
 
.tooltip[x-placement^="left"] .tooltip-arrow {
  border-width: 5px 0 5px 5px;
  border-top-color: transparent !important;
  border-right-color: transparent !important;
  border-bottom-color: transparent !important;
  right: -5px;
  top: calc(50% - 5px);
  margin-left: 0;
  margin-right: 0;
}
 
.tooltip.popover .popover-inner {
  background: #f9f9f9;
  color: black;
  padding: 24px;
  border-radius: 5px;
  box-shadow: 0 5px 30px rgba(black, .1);
}
 
.tooltip.popover .popover-arrow {
  border-color: #f9f9f9;
}
 
.tooltip[aria-hidden='true'] {
  visibility: hidden;
  opacity: 0;
  transition: opacity .15s, visibility .15s;
}
 
.tooltip[aria-hidden='false'] {
  visibility: visible;
  opacity: 1;
  transition: opacity .15s;
}
</style>