<template>
  <div class="list-model">
    <div class="table-box">
      <table>
        <tr>
          <th>
            <span>{{$t('#')}}</span>
          </th>
          <th>
            <span>{{$t('User')}}</span>
          </th>
          <th>
            <span>Token</span>
          </th>
          <th>
            <span>{{$t('Failure time')}}</span>
          </th>
          <th>
            <span>{{$t('Create Time')}}</span>
          </th>
          <th>
            <span>{{$t('Update Time')}}</span>
          </th>
          <th width="70">
            <span>{{$t('Operation')}}</span>
          </th>
        </tr>
        <tr v-for="(item, $index) in list" :key="$index">
          <td>
            <span>{{parseInt(pageNo === 1 ? ($index + 1) : (($index + 1) + (pageSize * (pageNo - 1))))}}</span>
          </td>
          <td>
            <span>
              <a href="javascript:" class="links">{{item.userName}}</a>
            </span>
          </td>
          <td><span>{{item.token}}</span></td>
          <td>
            <span>{{item.expireTime | formatDate}}</span>
          </td>
          <td><span>{{item.createTime | formatDate}}</span></td>
          <td><span>{{item.updateTime | formatDate}}</span></td>
          <td>
            <x-button type="info" shape="circle" size="xsmall" data-toggle="tooltip" icon="iconfont icon-bianjixiugai" :title="$t('Edit')" @click="_edit(item)">
            </x-button>
            <x-poptip
                    :ref="'poptip-delete-' + $index"
                    placement="bottom-end"
                    width="90">
              <p>{{$t('Delete?')}}</p>
              <div style="text-align: right; margin: 0;padding-top: 4px;">
                <x-button type="text" size="xsmall" shape="circle" @click="_closeDelete($index)">{{$t('Cancel')}}</x-button>
                <x-button type="primary" size="xsmall" shape="circle" @click="_delete(item,$index)">{{$t('Confirm')}}</x-button>
              </div>
              <template slot="reference">
                <x-button type="error" shape="circle" size="xsmall" data-toggle="tooltip" icon="iconfont icon-shanchu" :title="$t('delete')">
                </x-button>
              </template>
            </x-poptip>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>
<script>
  import { mapActions } from 'vuex'

  export default {
    name: 'token-list',
    data () {
      return {
        list: []
      }
    },
    props: {
      tokenList: Array,
      pageNo: Number,
      pageSize: Number
    },
    methods: {
      ...mapActions('user', ['deleteToken']),
      _closeDelete (i) {
        this.$refs[`poptip-delete-${i}`][0].doClose()
      },
      _delete (item, i) {
        this.deleteToken({
          id: item.id
        }).then(res => {
          this.$refs[`poptip-delete-${i}`][0].doClose()
          this.list.splice(i, 1)
          this.$message.success(res.msg)
        }).catch(e => {
          this.$refs[`poptip-delete-${i}`][0].doClose()
          this.$message.error(e.msg || '')
        })
      },
      _edit (item) {
        this.$emit('on-edit', item)
      }
    },
    watch: {
      tokenList (a) {
        this.list = []
        setTimeout(() => {
          this.list = a
        })
      }
    },
    created () {
      this.list = this.tokenList
    },
    mounted () {
    },
    components: { }
  }
</script>