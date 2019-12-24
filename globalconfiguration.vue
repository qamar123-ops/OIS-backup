<template>
  <div class="container-fluid content-main">
    <div class="content-header">
      <div class="ch-inner">
        <h2>Global Configurations</h2>
        <div class="breadcrumbs">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <a href="#">Home</a>
              </li>
              <li class="breadcrumb-item active" aria-current="page">Global Configurations</li>
            </ol>
          </nav>
        </div>
      </div>
    </div>
    <div class="content-container">
      <div class="container-fluid">
        <div class="card">
          <div class="card-body">
            <a class="btn btn-outline-dark">
              <span class="lnr lnr-trash"></span> Clear Cache
            </a>
            <!-- Button trigger modal -->
            <button
              class="btn btn-outline-dark add-data-btn"
              type="button"
              data-toggle="modal"
              data-target="#dataModal"
            >
              <span class="lnr lnr-file-add"></span> Add
            </button>
            <!-- Modal -->
            <div
              class="modal fade"
              id="dataModal"
              tabindex="-1"
              role="dialog"
              aria-labelledby="dataModalLabel"
              aria-hidden="true"
              ref="vuemodal"
            >
              <div class="modal-dialog" role="document">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">Manage Global Configuration</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                      <span aria-hidden="true">&times;</span>
                    </button>
                  </div>
                  <div class="modal-body">
                    <form>
                      <div class="form-group">
                        <label for="ConfigId">Config ID *</label>
                        <input
                          type="text"
                          class="form-control"
                          v-model="editConfiguration.ConfigId"
                          id="ConfigId"
                        />
                      </div>
                      <div class="form-group">
                        <label for="Value">Value *</label>
                        <div class="input-group mb-3">
                          <input
                            type="text"
                            class="form-control"
                            id="Value"
                            v-model="editConfiguration.Value"
                          />
                          <div class="input-group-append">
                            <span
                              class="input-group-text"
                              v-on:click="encryptValue()"
                              v-bind:class="{ 'fa fa-lock': isEncrypted, 'fa fa-unlock': !isEncrypted}"
                            ></span>
                          </div>
                        </div>
                      </div>
                      <div class="form-group">
                        <label for="Desc">Config Description</label>
                        <textarea
                          class="form-control"
                          id="Desc"
                          rows="3"
                          v-model="editConfiguration.Desc"
                        ></textarea>
                        <div class="form-check">
                          <input
                            type="checkbox"
                            v-model="editConfiguration.Active"
                            true-value="true"
                            false-value="false"
                          />
                        </div>
                      </div>
                    </form>
                  </div>
                  <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                    <button
                      type="button"
                      class="btn btn-primary"
                      v-on:click="postData(editConfiguration)"
                    >Save changes</button>
                  </div>
                </div>
              </div>
            </div>
            <div class="table-responsive pt-3">
              <table id="table_dt" class="table table-hover display">
                <thead class="blue-head">
                  <tr>
                    <th>ID</th>
                    <th>Value</th>
                    <th>Encrypted</th>
                    <th>Description</th>
                    <th>Active</th>
                    <th>Actions</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="objGlobalConfiguration in allConfigurations"
                    v-bind:key="objGlobalConfiguration.ConfigId"
                  >
                    <td>{{objGlobalConfiguration.ConfigId}}</td>
                    <td>{{objGlobalConfiguration.Value}}</td>
                    <td>{{ }}</td>
                    <td>{{objGlobalConfiguration.Desc}}</td>
                    <td>{{objGlobalConfiguration.Active}}</td>

                    <td>
                      <a
                        href="#"
                        data-toggle="modal"
                        data-target="#dataModal"
                        @click="editData(objGlobalConfiguration)"
                      >
                        <i class="fa fa-pencil-square-o mr-3 edit-action" aria-hidden="true"></i>
                      </a>
                      <a href="#" @click="deleteData(objGlobalConfiguration.ConfigId)">
                        <i class="fa fa-trash delete-action" aria-hidden="true"></i>
                      </a>
                    </td>
                  </tr>
                </tbody>
              </table>
              <div id="loading" v-bind:style="loading ? {display:'block'}: {display:'none'} "></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
module.exports = {
  data() {
    return {
      isEncrypted: false,
      Active: null,
      globalconfigurations: [],
      globalconfiguration: {
        ConfigId: "",
        Value: "",
        Desc: "",
        Active: null
      },
      loading: ""
    };
  },
  //====================LIFECYCLE HOOKS==============================
  beforeCreate() {},
  created() {
    this.fetchData();
  },
  mounted() {
    setTimeout(() => {
      this.loading = false;
    }, 500);
  },
  updated() {
    // $(".table-responsive #table_dt").DataTable();
    $(this.$refs.vuemodal).on("hidden.bs.modal", this.clearFields);
  },
  //====================COMPUTED PROPERTIES==============================
  computed: {
    allConfigurations() {
      return this.$store.getters.allConfigurations;
    },
    editConfiguration() {
      return this.$store.getters.editConfiguration;
    },
    getFormState() {
      return this.$store.getters.getIsAddState;
    }
  },
  //====================== METHODS=======================================
  methods: {
    // ========Get Data Method===========
     fetchData() {
      // debugger;
       this.$store.dispatch("fetchData");
    },
    // ========Post Data Method==========
    postData(globalconfiguration) {
      debugger;
      let vm = this;
      vm.globalconfiguration.Active = vm.editConfiguration.Active;
      vm.globalconfiguration.ConfigId = vm.editConfiguration.ConfigId;
      vm.globalconfiguration.Value = vm.editConfiguration.Value;
      vm.globalconfiguration.Desc = vm.editConfiguration.Desc;
      vm.editConfiguration.ConfigId = "";
      vm.editConfiguration.Value = "";
      vm.editConfiguration.Desc = "";
      vm.editConfiguration.Active = "";
      this.$store.dispatch("postData", this.globalconfiguration),
        $("#dataModal").modal("hide");
    },
    // ========Edit Data Method==========
    editData(postData) {
      this.$store.dispatch("editData", postData);
    },
    // ========Delete Data Method========
    deleteData(ConfigId) {
      let vm = this;
      Swal.fire({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        type: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!"
      }).then(result => {
        if (result.value) {
          console.log(result.value);
          vm.$store.dispatch("deleteData", ConfigId);
        }
      });
    },
    // ========Clear Modal Fields Method===========
    clearFields() {
      this.editConfiguration.ConfigId = "";
      this.editConfiguration.Value = "";
      this.editConfiguration.Desc = "";
      this.editConfiguration.Active = "";
    },
    // ================Encrypt Value Method===========
    encryptValue() {
      let encValue = this.editConfiguration.Value;
      var dataToSend = { value: encValue };
      if (!this.isEncrypted) {
        axios({
          method: "post",
          url: "/GlobalConfig/Encrypt",
          data: JSON.stringify(dataToSend),
          headers: {
            "Content-type": "application/json; charset=UTF-8"
          }
        })
          .then(response => {
            if (response.data != null) {
              this.editConfiguration.Value = response.data;
            }
          })
          .catch(function(response) {
            console.log(response, "exception");
          });
      } else {
        axios({
          method: "post",
          url: "/GlobalConfig/Decrypt",
          data: JSON.stringify(dataToSend),
          headers: {
            "Content-type": "application/json; charset=UTF-8"
          }
        })
          .then(response => {
            if (response.data != null) {
              this.editConfiguration.Value = response.data;
            }
          })
          .catch(function(response) {
            console.log(response, "exception");
          });
      }
      this.isEncrypted = !this.isEncrypted;
    }
  }
};
</script>

<style scoped>
.edit-action {
  color: green;
}
.delete-action {
  color: red;
}
.done {
  display: none;
}
</style>