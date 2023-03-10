<template>
  <div style="padding:12px">
    <v-row class="inspector-event-container-button-bar">
      <EventButtonBar/>
    </v-row>

    <v-divider class="border"/>
    <div class="inspector-event-split mt-2">
      <Split v-model="split" min="0px" max="0px">
        <div slot="left">
          <EventList class="inspector-event-left"></EventList>
        </div>
        <div slot="right">
          <EventDetail v-if="eventDetail" class="inspector-event-right"></EventDetail>
          <div v-else class="event-detail-empty">No selected event</div>
        </div>
      </Split>
    </div>
  </div>
</template>

<script>
import EventList from '@/views/event/EventList.vue'
import EventDetail from '@/views/event/EventDetail.vue'
import CodeEditor from '@/components/CodeEditor.vue'
import EventButtonBar from '@/views/event/EventButtonBar.vue'

export default {
  components: {
    EventList,
    EventDetail,
    CodeEditor,
    EventButtonBar
  },
  data () {
    return {
      scrollRate: 0,
      split: 1
    }
  },
  activated () {
    this.$bus.$on('eventListScroll', this.setEventContainerScroll)
  },
  deactivated () {
    this.$bus.$off('eventListScroll', this.setEventContainerScroll)
  },
  computed: {
    eventDetail () {
      return this.$store.state.event.eventDetail
    }
  },
  watch: {
    eventDetail (val) {
      if (!val) {
        this.split = 1
      } else if (this.split === 1) {
        this.split = 0.5
      } else { }
    }
  },
  methods: {
    addDescAttach (row) {
      if (this.channelAddToDesc.indexOf(row.channel) > -1) {
        this.dispatch('addIntoDesc', row.id)
      } else if (this.channelAddToAttach.indexOf(row.channel) > -1) {
        this.dispatch('addIntoAttach', row.id)
      }
      this.$Message.error(
        'Add description and attachments have not supported yet!'
      )
    },
    timestampToTime (timeStamp) {
      let date = new Date(timeStamp * 1000)
      let hour = date.getHours() + ':'
      let minute =
        (date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()) +
        ':'
      let second =
        date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds()
      return hour + minute + second
    },
    getColunmsFilterItem () {
      return []
    },
    setEventContainerScroll (event) {
      this.scrollRate = event
      setTimeout(() => {
        const container = this.$refs.eventContainer
        const topHeight = container.scrollHeight * event
        this.$refs.eventContainer.scrollTop = topHeight - (container.scrollHeight / 20)
      }, 1)
    }
  }
}
</script>

<style scoped>
.inspector-event-container-button-bar {
  height: 26px;
  display: flex;
  align-items: center;
  margin: 0px 0px 7px 0px !important;
}
.inspector-event-split {
  height: calc(100vh - 44px - 40px - 12px - 26px - 8px - 8px - 28px - 12px);
  /* total:100vh
  header: 44px
  title: 40px
  padding: 12px
  PaginationBar 32px
  buttonBar: 26px
  margin-bottom: 7px + line 1px = 8px
  table margin-top: 8px
  split
  margin-bottom: 12px
  footer: 28px
  */
}
.inspector-event-left {
  margin-right: 0px;
}
.inspector-event-right {
  height: calc(100vh - 44px - 40px - 12px - 26px - 8px - 8px - 28px - 12px);
  /* total:100vh
  header: 44px
  title: 40px
  padding: 12px
  PaginationBar 32px
  buttonBar: 26px
  margin-bottom: 7px + line 1px = 8px
  table margin-top: 8px
  split
  margin-bottom: 12px
  footer: 28px
  */
  margin-left: 5px;
}
.event-detail-empty {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}
</style>
