<template>
  <div>
    <div class="container border mt-5">
      <b-table striped hover :items="items" :fields="fields">
        <template #cell(#)="data">
            <button class="btn btn-success" @click="onEditForm(data.item)">
            Edit
          </button>
          <button class="btn btn-danger" @click="onDelete(data.item)">
            Delete
          </button>
        </template>
      </b-table>
    </div>
    <div class="container">
      <b-form @submit="onSubmit" @reset="onReset">
        <b-form-group id="input-group-1" label="Your Item:" label-for="input-1">
          <b-form-input id="input-1" v-model="form.nama_barang" placeholder="Enter Your Item" required>
          </b-form-input>
        </b-form-group>
        <b-form-group id="input-group-2" label="Items Price:" label-for="input-2">
          <b-form-input id="input-2" v-model="form.harga" placeholder="Enter Item's Price" required>
          </b-form-input>
        </b-form-group>
        <b-form-group id="input-group-3" label="Category" label-for="input-3">
          <b-form-input id="input-3" v-model="form.kategori" placeholder="Enter Item's Category" required>
          </b-form-input>
        </b-form-group>
        <b-button type="submit" variant="primary">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
      <div v-if="onEdit" class="mt-5">
        <b-form @submit="onSubmitEdit">
          <b-form-group id="input-group-1" label="Barang:" label-for="input-1">
            <b-form-input v-model="form_edit.nama_barang" value="form.nama_barang" placeholder="Enter Nama" required>
            </b-form-input>
          </b-form-group>
          <b-form-group id="input-group-2" label="Harga:" label-for="input-2">
            <b-form-input v-model="form_edit.harga" value="form.harga" placeholder="Enter Umur" required>
            </b-form-input>
          </b-form-group>
          <b-form-group id="input-group-3" label="kategori:" label-for="input-3">
            <b-form-input v-model="form_edit.kategori" value="form.kategori" placeholder="Enter Kategori" required>
            </b-form-input>
          </b-form-group>
            <b-button type="submit" variant="primary">Submit</b-button>
        </b-form>
      </div>
    </div>
  </div>

</template>

<script>
  export default {
    data() {
      return {
        items: [],
        form: {
          nama_barang: "",
          harga: "",
          kategori: ""
        },
        onEdit: false,
        form_edit: {
          nama_barang: "",
          harga: "",
          kategori: ""
        },
        fields: [{
            key: "id",
            sortable: true
          },
          {
            key: "nama_barang",
            sortable: true
          },
          {
            key: "harga",
            sortable: true
          },
          {
            key: "kategori",
            sortable: true
          },
          {
            key: "#",
            sortable: false
          },
        ],
        show: true
      }
    },
    methods: {
        async getapi() {
        const data = await this.$axios.$get("http://localhost:3500/users/gadget");
        this.items = data.data;
        for(let i = 0; i < this.items.length; i++){
          const element = this.items[i]
          element.id = i + 1
        }
        console.log(data.data);
      },
      async onDelete(data) {
        await this.$axios.$delete(
          `http://localhost:3500/users/gadget/` + data.id
        );
        this.getapi();
      },
      async onSubmit(event) {
        event.preventDefault();
        try {
          await this.$axios.$post(
            "http://localhost:3500/users/gadget",
            this.form
          );
          this.getapi();
        } catch (error) {
          console.log(error);
        }
      },
      onReset(event) {
        event.preventDefault();
        this.form.nama_barang = "";
        this.form.harga = "";
        this.form.kategori = "";
        this.show = false;
        this.$nextTick(() => {
          this.show = true;
        });
      },
      async onEditForm(data) {
        this.onEdit = true; 
        this.form_edit.nama_barang = data.nama_barang;
        this.form_edit.harga = data.harga;
        this.form_edit.kategori = data.kategori;
        this.form_edit.id = data.id;
      },
      async onSubmitEdit(event) {
        event.preventDefault();
        try{
            await this.$axios.$put(
              `http://localhost:3500/users/gadget/` + this.form_edit.id,
              this.form_edit
            );
            this.getapi();
        }
        catch(error){
          console.log(error);
        }
      },
    },

    mounted() {
      this.getapi();
    }
  }

</script>
