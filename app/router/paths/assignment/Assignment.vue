<script>
import AssignmentBase from '@/components/AssignmentBase.vue'
import throwable from '@/mixins/throwable'

import fileDialog from 'file-dialog'
import { assignment } from '@/src/api'
import dummy from './dummy'

const assignee = assignment.assignee

export default {
  name: 'Assignment',
  components: { AssignmentBase },
  mixins: [throwable],

  data: () => ({
    percentages: [],
    uploads: [],
    assignments: dummy
  }),

  methods: {
    async uploadFile (ass) {
      const files = await fileDialog()
      this.$set(this.uploads, ass.idx, true)
      try {
        await assignee.submitAssignment(ass.idx, files[0], event =>
          this.$set(this.percentages, ass.idx, Math.floor((event.loaded * 100) / event.total)))
      } catch (err) {
        this.$_throwable_handleError(err)
      } finally {
        this.uploads.splice(ass.idx, 1)
        this.$set(this.percentages, ass.idx, 0)
      }
    }
  }
}
</script>

<template>
  <assignment-base :assignments="assignments">
    <template slot-scope="{ ass }">
      <span
        class="assignee__upload"
        @click="uploadFile(ass)">
        <span class="icon-upload"/> {{
          uploads[ass.idx] ? (percentages[ass.idx] || 0) + '%' :
          ass.report ? '수정' : '제출'
        }}
      </span>
    </template>

    <span slot="header">
      <span class="icon-submission"/> 과제 제출
    </span>

    <span
      slot="opponent"
      slot-scope="{ ass }">
      {{ ass.assignor.name }}
    </span>

    <span
      slot="badge"
      slot-scope="{ ass }">
      <dimi-badge
        :color="ass.report ? 'aloes' : 'orange'"
        class="assignee__badge">
        <span :class="ass.report ? 'icon-ok' : 'icon-cross'"/>
        {{ ass.report ? '제출' : '미제출' }}
      </dimi-badge>
    </span>
  </assignment-base>
</template>

<style lang="scss">
  .assignee {
    &__upload {
      cursor: pointer;
    }

    &__badge {
      margin-right: 16px;
    }
  }
</style>
