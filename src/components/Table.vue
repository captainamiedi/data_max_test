<template>
  <div class="hello">
    <div class="search">
      <div class="search_text">
      <p>Search:</p>
      </div>
        <input type="text" v-model="search" placeholder="Search by name">
    </div>
    <table class="table table-bordered table-striped">
  <thead>
    <tr>
      <!-- <th scope="col"><th> -->
      <th scope="col">s/n</th>
      <th scope="col">Name</th>
      <th scope="col">ISBN</th>
      <th scope="col">Authors</th>
      <th scope="col">Number of Pages</th>
      <th scope="col">Country</th>
      <th scope="col">Released</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(item, index) in data" :key="index">
      <td>{{index + 1}}</td>
      <td>{{item.name}}</td>
      <td>{{item.isbn}}</td>
      <td >
        <span v-for="author in item.authors" :key="author">
        {{author}},
        </span>
      </td>
      <td>{{item.numberOfPages}}</td>
      <td>{{item.country}}</td>
      <td>{{new Date(item.released).toLocaleDateString("en-US")}}</td>
    </tr>
  </tbody>
</table>
<!-- the endpoint is not returning the total number of entries -->
<div class="pagination-container">
  <p>showing {{pageNum}} to 10 of 12 entries</p>
<nav aria-label="Page navigation example">
  <ul class="pagination justify-content-end">
    <li class="page-item" v-bind:class="{ disabled: pageNum === 1 }" @click="changePageNumber(pageNum - 1)">
      <a class="page-link" href="#" tabindex="-1" aria-disabled="true">Previous</a>
    </li>
    <li class="page-item" v-bind:class="{ disabled: pageNum === 1 }" @click="changePageNumber(1)"><a class="page-link" href="#">1</a></li>
    <li class="page-item" v-bind:class="{ disabled: pageNum === 2 }" @click="changePageNumber(2)"><a class="page-link" href="#">2</a></li>
    <li class="page-item" v-bind:class="{ disabled: pageNum === 2 }" @click="changePageNumber(pageNum + 1)">
      <a class="page-link" href="#">Next</a>
    </li>
  </ul>
</nav>
</div>
  </div>
</template>

<script>

export default {
  name: 'HelloWorld',
  data(){
    return {
      data:  [],
      search: '',
      pageNum: 1
    }
  },
  created(){
    fetch(`https://www.anapioficeandfire.com/api/books?page=${this.pageNum}&pageSize=10`).then(response => response.json())
    .then(res => {
      console.log(res, 'resp');
      this.data = res
    });
  },
 watch: {
    search: function() {
      this.filterByName(this.search)
      console.log(this.search)
    },
    pageNum: function() {
      this.getBooks()
    }
  },
  methods: {
    filterByName: function(val) {
      fetch(`https://www.anapioficeandfire.com/api/books?name=${val}`).then(response => response.json())
    .then(res => {
      console.log(res, 'resp');
      this.data = res
    });
    },
    changePageNumber: function (value){
      console.log(value, 'value');
      console.log(this.pageNum, 'pagenum');
      if (this.pageNum < 0 ){
        return null
      }
      if (this.pageNum > 2) {
        return null
      }
      this.pageNum = value
    },
    getBooks: function(){
      fetch(`https://www.anapioficeandfire.com/api/books?page=${this.pageNum}&pageSize=10`).then(response => response.json())
        .then(res => {
      console.log(res, 'resp');
      this.data = res
    });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.search_text{
  /* padding: 10px 5px 0px 0px; */
  padding-right: 8px;
  /* margin-top: 5px; */
}
p{
  margin-top: 10px;
}
.hello{
  padding: 0px 20px;
}
.search{
  display: flex;
  justify-content: flex-end;
  padding: 10px 0px;
}
.pagination-container{
  display: flex;
  justify-content: space-between;
}
</style>
