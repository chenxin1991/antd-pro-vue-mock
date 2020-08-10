<template>
  <a-card :bordered="false">
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :md="6" :sm="24">
            <a-form-item label="角色名">
              <a-input v-model="queryParam.id" placeholder=""/>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="24">
            <span class="table-page-search-submitButtons">
              <a-button type="primary" @click="$refs.table.refresh(true)">查询</a-button>
              <a-button style="margin-left: 8px" @click="() => queryParam = {}">重置</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <div class="table-operator">
      <a-button type="primary" icon="plus" @click="$refs.roleForm.add()">新建</a-button>
    </div>
    <s-table
      ref="table"
      size="small"
      rowKey="key"
      :columns="columns"
      :data="loadData"
    >
      <span slot="action" slot-scope="text, record">
        <template>
          <a @click="handleEdit(record)">编辑</a>
          <a-divider type="vertical" />
          <a @click="handleDelete(record)">删除</a>
        </template>
      </span>
    </s-table>
    <role-form ref="roleForm" @ok="handleOk" />
  </a-card>
</template>

<script>
import { STable, Tree } from '@/components'
import RoleForm from './RoleForm'
import { getRoleList, getServiceList } from '@/api/manage'

export default {
  name: 'SettingRole',
  components: {
    STable,
    RoleForm,
    Tree
  },
  data () {
    return {
      // 查询参数
      queryParam: {},
      // 表头
      columns: [
        {
          title: '角色名',
          dataIndex: 'no'
        },
        {
          title: '操作',
          dataIndex: 'action',
          width: '150px',
          scopedSlots: { customRender: 'action' }
        }
      ],
      // 加载数据方法 必须为 Promise 对象
      loadData: parameter => {
        console.log('loadData.parameter', parameter)
        return getServiceList(Object.assign(parameter, this.queryParam))
          .then(res => {
            return res.result
          })
      }
    }
  },
 created () {
  getRoleList({ t: new Date() })
 },
  methods: {
    handleEdit (record) {
      this.$refs.roleForm.edit(record)
    },
    handleDelete (record) {
    },
    handleOk () {
      this.$refs.table.refresh()
    }
  }
}
</script>
