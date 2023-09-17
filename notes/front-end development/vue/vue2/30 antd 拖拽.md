```bash
npm i vue-draggable-resizable
# 如果有冲突，加上 --legacy-peer-deps
npm i vue-draggable-resizable --legacy-peer-deps
npm uninstall vue-draggable-resizable --legacy-peer-deps
```

```js
import VueDraggableResizable from 'vue-draggable-resizable'
Vue.component('vue-draggable-resizable', VueDraggableResizable)
```

`draggable-resizable.js`：

```js
import Vue from 'vue'
export const dragResizableFunc = (columns) => {
  const draggingMap = {}
  columns.forEach((col) => {
    const k = col.dataIndex || col.key
    draggingMap[k] = col.width
  })
  const draggingState = Vue.observable(draggingMap)
  return (h, props, children) => {
    let thDom = null
    const { key, ...restProps } = props
    let col = { width: 40 }
    if (key === 'selection-column') {
      //支持复选框
      col = {
        dataIndex: 'selection-column',
        key: 'selection-column',
        width: 40
      }
    } else {
      col = columns.find((col) => {
        const k = col.dataIndex || col.key
        return k === key
      })
    }

    if (!col.width) {
      return <th {...restProps}>{children}</th>
    }
    const onDrag = (x) => {
      draggingState[key] = 0
      col.width = Math.max(x, 1)
    }

    const onDragstop = () => {
      draggingState[key] = thDom.getBoundingClientRect().width
    }
    return (
      <th
        {...restProps}
        v-ant-ref={(r) => (thDom = r)}
        width={draggingState[key]}
        class="resize-table-th"
      >
        {children}
        <vue-draggable-resizable
          key={col.dataIndex || col.key}
          class="table-draggable-handle"
          w={10}
          x={draggingState[key] || col.width}
          z={1}
          axis="x"
          draggable={true}
          resizable={false}
          onDragging={onDrag}
          onDragstop={onDragstop}
        ></vue-draggable-resizable>
      </th>
    )
  }
}
```

使用：

```vue
<template>
  <a-table
    bordered
    :columns="columns"
    :components="components"
    :row-selection="rowSelection"
    :data-source="data"
  >
    <template v-slot:action>
      <a href="javascript:;">Delete</a>
    </template>
  </a-table>
</template>

<script>
import { dragResizableFunc } from '@/utils/draggable-resizable'

export default {
  name: 'App',
  data() {
    return {
      rowSelection: {
        onChange: (selectedRowKeys, selectedRows) => {
          console.log(
            `selectedRowKeys: ${selectedRowKeys}`,
            'selectedRows: ',
            selectedRows
          )
        },
        onSelect: (record, selected, selectedRows) => {
          console.log(record, selected, selectedRows)
        },
        onSelectAll: (selected, selectedRows, changeRows) => {
          console.log(selected, selectedRows, changeRows)
        }
      },
      data: [
        {
          key: 0,
          date: '2018-02-11',
          amount: 120,
          type: 'income',
          note: 'transfer'
        },
        {
          key: 1,
          date: '2018-03-11',
          amount: 243,
          type: 'income',
          note: 'transfer'
        },
        {
          key: 2,
          date: '2018-04-11',
          amount: 98,
          type: 'income',
          note: 'transfer'
        }
      ],
      columns: [
        {
          title: 'Date',
          dataIndex: 'date',
          width: 200
        },
        {
          title: 'Amount',
          dataIndex: 'amount',
          width: 100
        },
        {
          title: 'Type',
          dataIndex: 'type',
          width: 100
        },
        {
          title: 'Note',
          dataIndex: 'note',
          scopedSlots: { customRender: 'note' },
          width: 100
        },
        {
          title: 'Action',
          key: 'action',
          scopedSlots: { customRender: 'action' }
        }
      ],
      components: null
    }
  },
  created() {
    this.components = {
      header: {
        cell: dragResizableFunc(this.columns)
      }
    }
  }
}
</script>
<style lang="less" scoped>
/deep/ .resize-table-th {
  position: relative;
  .table-draggable-handle {
    height: 100% !important;
    bottom: 0;
    left: auto !important;
    right: -5px;
    cursor: col-resize;
    touch-action: none;
    transform: none !important;
    position: absolute;
  }
}
</style>
```

