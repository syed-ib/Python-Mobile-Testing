<template>
  <div v-if="jsonPath">
    <Tooltip
      :content="jsonPath"
      placement="bottom-end"
      theme="light"
      max-width="500"
      transfer
      style="width:100%"
    >
      <span class="json-path-span">
        <b style="padding-right:5px">JSONPath:</b>{{jsonPath}}
      </span>
    </Tooltip>
  </div>
</template>

<script>
export default {
  computed: {
    jsonPath() {
      return this.$store.state.dataManager.jsonPath
    }
  }
}
</script>

<style>
.ivu-tooltip-inner-with-width{
  word-break: break-all;
  white-space: normal;
  width: 300px;
}
.json-path-span{
  display: inline-block;
  width: 100%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  vertical-align: top;
  cursor: pointer;
}
</style>
