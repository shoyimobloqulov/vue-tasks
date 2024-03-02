<template>
  <div class="ml-5 mt-5">
    <div class="row">
      <div class="col-md-3">
        <div class="form-group">
          <label for="">Viloyat</label>
          <select class="form-control select2" v-model="region" @change="districtSelect">
            <option v-for="x in valueProvince" v-bind:value="x.id">{{ x.name }}</option>
          </select>
        </div>
      </div>
      <div class="col-md-3">
        <div class="form-group">
          <label for="">Tuman</label>
          <select class="form-control select2" v-model="district">
            <option v-for="x in valueDistrict" v-bind:value="x.id">{{ x.name }}</option>
          </select>
        </div>
      </div>

      <div class="col-md-3">
        <div class="form-group">
          <label for="comment">Comment</label>
          <input type="text" id="comment" v-model="comment" class="form-control">
        </div>
      </div>

      <div class="col-md-3">
        <button class="btn btn-success btn-sm" @click="AddDataList"><i class="fa fa-product-hunt"></i> Qo'shish</button>
      </div>
    </div>
    <p class="text-danger">{{ this.error }}</p>
    <table class="table">
      <thead>
      <tr>
        <th scope="col">Viloyat</th>
        <th scope="col">Tuman</th>
        <th scope="col">Comment</th>
        <th scope="col">Amallar</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(x,index) in DataList" :key="index">
        <td>{{ x.region.name }}</td>
        <td>{{ x.district.name }}</td>
        <td>{{ x.comment }}</td>
        <td>
          <button class="btn btn-danger btn-sm" @click="deleteDataList(index)"><i class="fa fa-trash"></i></button>
          <button class="btn btn-info btn-sm" @click="editData(index)"><i class="fa fa-edit"></i></button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>

</template>

<script>
export default {
  data() {
    return {
      valueProvince: [],
      valueDistrict: [],
      DataList: [],
      comment: '',
      district: '',
      region: '',
      error: ''
    };
  },
  async mounted() {
    const response = await fetch('https://robocontest.uz/api/regions');
    this.valueProvince = await response.json()
  }
  ,
  methods : {

    AddDataList() {
      const selectedRegion = this.valueProvince.find((item) => item.id === this.region);
      const selectedDistrict = this.valueDistrict.find((item) => item.id === this.district);
      if (this.region === 0) {
        this.error = "Viloyat kiritishingiz kerak"
        return ;
      }
      if (this.district === 0) {
        this.error = "Tuman kiritishingiz kerak"
        return ;
      }
      if (this.comment === '') {
        this.error = "Comment kiritishingiz kerak"
        return ;
      }

      this.DataList.push({
        region: {
          id: selectedRegion.id,
          name: selectedRegion.name
        },
        district: {
          id: selectedDistrict.id,
          name: selectedDistrict.name
        },
        comment: this.comment
      });
    },
    editData(index){
      const element = this.DataList[index]
      element.$set(this.DataList, this.editingIndex, {
        region: {
          id: selectedRegion.id,
          name: selectedRegion.name
        },
        district: {
          id: selectedDistrict.id,
          name: selectedDistrict.name
        },
        comment: this.comment
      })
      console.log(element)
    },
    async districtSelect() {
      let region_id = this.region
      const response = await fetch('https://robocontest.uz/api/regions?q=' + region_id);
      this.valueDistrict = await response.json()
    },
    deleteDataList(id = 0){
      this.DataList.splice(id,1);
    }
  }
};
</script>