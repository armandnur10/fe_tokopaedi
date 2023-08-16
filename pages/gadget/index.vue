<template>
  <div>
    <div>
      <b-navbar toggleable="sm" type="light" variant="light">
        <b-navbar-toggle target="nav-text-collapse"></b-navbar-toggle>

        <b-navbar-brand>TokoPaedi</b-navbar-brand>

        <b-collapse id="nav-text-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-text>
              <a href="http://localhost:3000/gadget" style="text-decoration: none;color: rgba(0, 0, 0, 0.725);">Daftar Barang</a>
            </b-nav-text>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>
    <div class="container ">

      <div class="row">
        <div class="col-md-6">
          <h2 class="my-4">Tambahkan Barang</h2>
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
              <b-form-select v-model="form.kategori" :options="options">
              </b-form-select>
            </b-form-group>

            <b-form-group id="input-group-4" label="Image " label-for="input-4">
              <b-form-input id="input-4" v-model="form.gambar"
                placeholder=" Image(Only link Image/not accept local image)" required>
              </b-form-input>
            </b-form-group>

            <b-button type="submit" variant="primary">Submit</b-button>
            <b-button type="reset" variant="danger">Reset</b-button>
          </b-form>
        </div>

        <div class="col-md-6">
          <div v-if="onEdit">
            <h2 class="my-4">Edit Barang</h2>
            <b-form @submit="onSubmitEdit" @reset="onCancelEdit">
              <b-form-group id="input-group-1" label="Your Edited Item Name:" label-for="input-1">
                <b-form-input v-model="form_edit.nama_barang" value="form.nama_barang" placeholder="Enter Nama"
                  required>
                </b-form-input>
              </b-form-group>

              <b-form-group id="input-group-2" label="Price:" label-for="input-2">
                <b-form-input v-model="form_edit.harga" value="form.harga" placeholder="Enter Umur" required>
                </b-form-input>
              </b-form-group>

              <b-form-group id="input-group-3" label="Category:" label-for="input-3">
                <b-form-select v-model="form_edit.kategori" value="form.kategori" :options="options">
                </b-form-select>
              </b-form-group>

              <b-form-group id="input-group-4" label="Image" label-for="input-4">
                <b-form-input id="input-4" v-model="form_edit.gambar" value="form.gambar"
                  placeholder=" Image(Only link Image/not accept local image)" required>
                </b-form-input>
              </b-form-group>

              <b-button type="submit" variant="primary">Submit</b-button>
              <b-button type="reset" variant="danger">Cancel</b-button>
            </b-form>
          </div>
        </div>
      </div>

    </div>
    <div class="container mt-5">

      <h2 class="my-4">List Barang</h2>
      <b-table striped hover :items="items" class="border" :fields="fields">
        <template #cell(image)="row">
          <img :src="row.item.gambar" class="obj-cover rounded" width="50px" height="50px" alt="">
        </template>
        <template #cell(#)="data">
          <button class="btn btn-success" @click="onEditForm(data.item)">
            Edit
          </button>
          <button class="btn btn-danger" @click="onDelete(data.item)">
            Delete
          </button>
          <button class="btn btn-warning" @click="goToDetail(data.item.id)">Detail</button>
        </template>
      </b-table>
    </div>
  </div>

</template>

<script>
  export default {
    name: 'Index',
    data() {
      return {
        items: [],
        form: {
          nama_barang: "",
          harga: "",
          kategori: "",
          gambar: ""
        },
        options: [{
            value: 'Smartphone',
            text: 'Smartphone'
          },
          {
            value: 'Tablet',
            text: 'Tablet'
          },
          {
            value: 'Laptop',
            text: 'Laptop'
          },
          {
            value: 'Other',
            text: 'Other'
          },
        ],
        onEdit: false,
        form_edit: {
          nama_barang: "",
          harga: "",
          kategori: "",
          gambar: ""
        },
        fields: [{
            key: "image",
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
        this.form_edit.gambar = data.gambar;
        this.form_edit.id = data.id;
      },
      async onCancelEdit(event) {
        event.preventDefault();
        this.onEdit = false;
      },
      async onSubmitEdit(event) {
        event.preventDefault();
        try {
          await this.$axios.$put(
            `http://localhost:3500/users/gadget/` + this.form_edit.id,
            this.form_edit
          );
          this.getapi();
        } catch (error) {
          console.log(error);
        }
      },
      async goToDetail(id) {
        this.$router.push(`gadget/detail/${id}`);
      },
    },

    mounted() {
      this.getapi();
    }
  }

</script>
