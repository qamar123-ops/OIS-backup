<template>
  <div class="container-fluid content-main">
    <div class="content-header">
      <div class="ch-inner">
        <h2>Application Configurations</h2>
        <div class="breadcrumbs">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <a href="#">Home</a>
              </li>
              <li class="breadcrumb-item active" aria-current="page">Application Configurations</li>
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
              data-target="#appDataModal"
            >
              <span class="lnr lnr-file-add"></span> Add
            </button>
            <div class="input-group mb-3 app-config-dropdown">
              <select
                name="getapps"
                class="custom-select"
                id="getapps"
                v-model="editApplicationConfiguration.AppId"
                @change="onApplicationChange(editApplicationConfiguration.AppId)"
              >
                <option value selected="selected">Choose Application...</option>
                <option
                  v-for="appData in  getAllApplications"
                  v-bind:key="appData.AppId"
                  :value="appData.AppId"
                >{{ appData.Name }}</option>
              </select>
            </div>
            <!-- Modal -->
            <div
              class="modal fade"
              id="appDataModal"
              tabindex="-1"
              role="dialog"
              aria-labelledby="appDataModalLabel"
              aria-hidden="true"
              ref="vuemodal"
            >
              <div class="modal-dialog" role="document">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="AppDataModalLabel">Manage Application Configuration</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                      <span aria-hidden="true">&times;</span>
                    </button>
                  </div>
                  <div class="modal-body">
                    <form>
                      <input type="hidden" class="form-control" id="AppId" />
                      <input type="hidden" class="form-control" id="Type" />
                      <div class="form-group">
                        <label for="ConfigId">Config ID *</label>
                        <input
                          type="text"
                          class="form-control"
                          v-model="editApplicationConfiguration.ConfigId"
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
                            v-model="editApplicationConfiguration.Value"
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
                          v-model="editApplicationConfiguration.Desc"
                        ></textarea>
                        <div class="form-check">
                          <input
                            type="checkbox"
                            v-model="editApplicationConfiguration.Active"
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
                      v-on:click="postData(editApplicationConfiguration)"
                    >Save changes</button>
                  </div>
                </div>
              </div>
            </div>
            <div class="table-responsive pt-3">
              <table id="applicationconfiguration_table" class="table table-hover display">
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
                    v-for="objApplicationConfiguration in allApplicationConfiguration"
                    v-bind:key="objApplicationConfiguration.ConfigId"
                  >
                    <td>{{objApplicationConfiguration.ConfigId}}</td>
                    <td>{{objApplicationConfiguration.Value}}</td>
                    <td>{{ }}</td>
                    <td>{{objApplicationConfiguration.Desc}}</td>
                    <td>{{objApplicationConfiguration.Active}}</td>
                    <td>
                      <a
                        href="#"
                        data-toggle="modal"
                        data-target="#appDataModal"
                        @click="editData(objApplicationConfiguration)"
                      >
                        <i class="fa fa-pencil-square-o mr-3 edit-action" aria-hidden="true"></i>
                      </a>
                      <a href="#" @click="deleteData(objApplicationConfiguration)">
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
      applicationconfigurations: [],
      applicationconfiguration: {
        ConfigId: "",
        Value: "",
        Desc: "",
        Active: null,
        Type: null,
        AppId: ""
      },
      loading: ""
    };
  },
  ready() {},
  //====================LIFECYCLE HOOKS==============================
  beforeCreate() {},
  created() {
    this.selectApps();
  },
  mounted() {
    setTimeout(() => {
      this.loading = false;
    }, 500);
  },
  updated() {
    // this.$nextTick(function() {
    //   $("#app-datatable").DataTable();
    // });
    // $(".table-responsive #applicationconfiguration_table").DataTable();
    $(this.$refs.vuemodal).on("hidden.bs.modal", this.clearFields);
  },
  //====================COMPUTED PROPERTIES==============================
  computed: {
    allApplicationConfiguration() {
      return this.$store.getters.allApplicationConfiguration;
    },
    editApplicationConfiguration() {
      return this.$store.getters.editApplicationConfiguration;
    },
    getFormState() {
      return this.$store.getters.getIsAddState;
    },
    getAllApplications() {
      return this.$store.getters.getAllApplications;
    }
  },
  //====================== METHODS=======================================
  methods: {
    // ========Get Data Method===========
    onApplicationChange(AppId) {
      this.$store.dispatch("onApplicationChange", AppId);
    },
    // ========Post Data Method==========
    postData(applicationconfiguration) {
      let vm = this;
      vm.applicationconfiguration.Active =
        vm.editApplicationConfiguration.Active;
      vm.applicationconfiguration.ConfigId =
        vm.editApplicationConfiguration.ConfigId;
      vm.applicationconfiguration.Value = vm.editApplicationConfiguration.Value;
      vm.applicationconfiguration.Desc = vm.editApplicationConfiguration.Desc;
      vm.applicationconfiguration.AppId = vm.editApplicationConfiguration.AppId;
      vm.editApplicationConfiguration.ConfigId = "";
      vm.editApplicationConfiguration.Value = "";
      vm.editApplicationConfiguration.Desc = "";
      vm.editApplicationConfiguration.Active = "";
      vm.editApplicationConfiguration.AppId = "";
      this.$store.dispatch("postData", this.applicationconfiguration),
        $("#appDataModal").modal("hide");
    },
    // ========Edit Data Method==========
    editData(postData) {
      this.$store.dispatch("editData", postData);
    },
    // ========Delete Data Method========
    deleteData(postData) {
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
          vm.$store.dispatch("deleteData", postData);
        }
      });
    },
    selectApps() {
      this.$store.dispatch("selectApps");
    },
    // ========Clear Modal Fields Method===========
    clearFields() {
      this.editApplicationConfiguration.ConfigId = "";
      this.editApplicationConfiguration.Value = "";
      this.editApplicationConfiguration.Desc = "";
      this.editApplicationConfiguration.Active = "";
    },
    // ================Encrypt Value Method===========
    encryptValue() {
      let encValue = this.editApplicationConfiguration.Value;
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
              this.editApplicationConfiguration.Value = response.data;
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
              this.editApplicationConfiguration.Value = response.data;
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
.app-config-dropdown {
  display: inline-flex;
  width: 15%;
  float: right;
}
</style>