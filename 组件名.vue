<template>
    <div>
        <!-- 分页 -->
        <Pagination
          :pageNo="page"
          :pageSize="limit"
          :total="myOrder.total"
          :continues="5"
          @getPageNo="getPageNo"
        ></Pagination>
    </div>
  </template>
  <script>
  export default {
  
  }
  </script>
  
  <style>
  
  </style>