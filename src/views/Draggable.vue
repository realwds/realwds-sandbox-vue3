<template>
  <el-row :gutter="20">
    <el-col :span="18">
      <el-table :data="tableData" current-row-key="id" border>
        <el-table-column prop="date" label="日期" width="180"></el-table-column>
        <el-table-column label="姓名" width="180">
          <template v-slot="scope">
            <draggable
              :list="scope.row.name"
              ghost-class="ghost"
              :force-fallback="true"
              animation="300"
              group="itxst"
              @add="addarr2(scope.row.id, scope.row.name)"
            >
              <template #item="{ element }">
                <el-tag
                  :key="element.id"
                  closable
                  :disable-transitions="false"
                  @close="handleRemove(scope.row.name, element)"
                >
                  {{ element.value }}
                </el-tag>
              </template>
            </draggable>
            <el-autocomplete
              class="input-new-tag"
              ref="saveTagInput"
              size="small"
              v-if="scope.row.inputVisible"
              v-model="scope.row.inputValue"
              :fetch-suggestions="querySearch"
              clearable
              @trigger-on-focus="false"
              @select="handleSelect($event, scope.row.name, scope.row.id)"
            ></el-autocomplete>
          </template>
        </el-table-column>
        <el-table-column prop="address" label="地址"></el-table-column>
      </el-table>
    </el-col>
    <el-col :span="6">
      <draggable
        :list="arr1"
        ghost-class="ghost"
        :force-fallback="true"
        animation="300"
        :group="groupA"
        sort="false"
      >
        <template #item="{ element }">
          <el-tag>
            {{ element.value }}
          </el-tag>
        </template>
      </draggable>
    </el-col>
  </el-row>
</template>

<script setup>
import draggable from "vuedraggable";

const arr1 = [
  { id: 1, value: "王小虎" },
  { id: 2, value: "王二虎" },
  { id: 3, value: "张三" },
  { id: 4, value: "赵四" },
  { id: 5, value: "一甲" },
];

const tableData = [
  {
    id: "1",
    date: "2022-08-02",
    name: [
      { id: 1, value: "王小虎" },
      { id: 2, value: "王二虎" },
    ],
    address: "深圳市龙岗坂田街道21号",
    inputValue: "",
    inputVisible: false,
  },
  {
    id: "2",
    date: "2022-08-04",
    name: [],
    address: "深圳市龙岗坂田街道22号",
    inputValue: "",
    inputVisible: false,
  },
  {
    id: "3",
    date: "2022-08-01",
    name: [],
    address: "深圳市龙岗坂田街道21号",
    inputValue: "",
    inputVisible: false,
  },
  {
    id: "4",
    date: "2022-08-03",
    name: [],
    address: "深圳市龙岗坂田街道21号",
    inputValue: "",
    inputVisible: false,
  },
];

let moveId = -1;
let groupA = {
  name: "itxst",
  pull: "clone",
  put: false,
};

const loadAll = function () {
  return arr1;
};

let restaurants = loadAll;

//左边往右边拖动时的事件
const addarr2 = function (_id, _data, e) {
  const map = new Map();
  const qc = _data.filter((item) => !map.has(item.id) && map.set(item.id, 1));
  for (var i = 0; i < tableData.length; i++) {
    if (tableData[i].id == _id) {
      tableData[i].name = qc;
      break;
    }
  }
};
const handleRemove = function (datalist, tag) {
  datalist.splice(datalist.indexOf(tag), 1);
};

const showInput = function (_id) {
  console.log(_id);
  for (var i = 0; i < tableData.length; i++) {
    if (tableData[i].id == _id) {
      tableData[i].inputVisible = true;
      break;
    }
  }
};
const querySearch = function (queryString, cb) {
  var restaurants = restaurants;
  var results = queryString
    ? restaurants.filter(createFilter(queryString))
    : restaurants;
  cb(results);
};
const createFilter = function (queryString) {
  return (restaurant) => {
    return (
      restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
    );
  };
};
const handleSelect = function (item, datalist, _id) {
  if (item) {
    datalist.push(item);
    const map = new Map();
    const qc = datalist.filter(
      (item) => !map.has(item.id) && map.set(item.id, 1)
    );
    for (var i = 0; i < tableData.length; i++) {
      if (tableData[i].id == _id) {
        tableData[i].name = qc;
        ttableData[i].inputValue = "";
        tableData[i].inputVisible = false;
        break;
      }
    }
  }
};
</script>
