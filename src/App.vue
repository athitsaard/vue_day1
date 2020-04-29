<template>
    <div>
        <div class="form-wrapper">
            <form @submit.prevent="saveItemData">
                <div>
                    <label for="itemType">ItemType:</label>
                    <input type="text" name="itemType" v-model="form.itemType" />
                </div>
                <div>
                    <label for="itemDescription">ItemDescription:</label>
                    <textarea name="description" id="description" cols="40" rows="5" v-model="form.itemDescription"></textarea>
                </div>
                <div>
                    <label for="itemFine">Fine:</label>
                    <input type="text" name="itemFine" v-model="form.itemFine">
                </div>
                <div>
                    <label for="itemIsPaid">Is Paid:</label>
                    <input type="radio" id="true" name="isPaid" value="Yes" v-model="form.itemIsPaid">
                    <label for="male"> Yes </label>
                    <input type="radio" id="false" name="isPaid" value="No" v-model="form.itemIsPaid">
                    <label for="female">No</label>
                </div>
                <input type="submit" :value="submitButton">
                <input type="button" @click="clearForm" value="Cancel">
            </form>   
        </div>
        <div class="data-wrapper">
            <table v-if="items.length > 0">
                <thead>
                    <td>Type</td>
                    <td>Description</td>
                    <td>Fine</td>
                    <td>Is Paid</td>
                    <td></td>
                </thead>
                <tr v-for="(item) in items" :key="item.itemID">
                    <td>{{ item.itemType}}</td>
                    <td>{{ item.itemDescription}}</td>
                    <td>{{ item.itemFine}}</td>
                    <td>{{ item.itemIsPaid}}</td>
                    <td>
                        <button @click="updateItem(item.itemID)">edit</button>
                        <button @click="removeItem(item.itemID)">Delete</button>
                    </td>
                </tr>
            </table>
            <p v-else style="color:red">!Not have Data</p>
        </div>
    </div>
</template>

<script>
export default {
  data: function() {
    return {
      form: {
        itemID: null,
        itemType: "",
        itemDescription: "",
        itemFine: 0,
        itemIsPaid: false
      },
      items: []
    };
  },
  computed: {
    submitButton() {
      if (this.form.itemID) {
        return "Update";
      } else {
        return "Add";
      }
    }
  },
  methods: {
    validate() {
      const isTypeExist = this.form.itemType.length > 0;
      const isDescriptionExist = this.form.itemDescription.length > 0;
      const isFineExist = this.form.itemFine.length > 0;
      return isTypeExist && isDescriptionExist && isFineExist;
    },
    saveItemData() {
      if (!this.validate()) {
        alert("Please Fill All Input. And check if Fine is more than 0");
        return;
      }
      if (this.form.itemID) {
        for (let i = 0; i < this.items.length; i++) {
          if (this.items[i].itemID === this.form.itemID) {
            this.items[i].itemType = this.form.itemType;
            this.items[i].itemDescription = this.form.itemDescription;
            this.items[i].itemFine = this.form.itemFine;
            this.items[i].itemIsPaid = this.form.itemIsPaid;
            this.clearForm();
            break;
          }
        }
      } else {
        const newItem = {
          itemID: Math.floor(Math.random() * 1000),
          itemType: this.form.itemType,
          itemDescription: this.form.itemDescription,
          itemFine: this.form.itemFine,
          itemIsPaid: this.form.itemIsPaid
        };
        this.items.push(newItem);
        this.clearForm();
      }
    },
    clearForm() {
      this.form = {
        itemID: null,
        itemType: "",
        itemDescription: "",
        itemFine: 0,
        itemIsPaid: false
      };
    },
    updateItem(id) {
      for (let i = 0; i < this.items.length; i++) {
        if (this.items[i].itemID === id) {
          this.form.itemID = this.items[i].itemID;
          this.form.itemType = this.items[i].itemType;
          this.form.itemDescription = this.items[i].itemDescription;
          this.form.itemFine = this.items[i].itemFine;
          this.form.itemIsPaid = this.items[i].itemIsPaid;
          break;
        }
      }
    },
    removeItem(id) {
      this.items = this.items.filter(i => i.itemID !== id);
    }
  }
};
</script>

<style lang="scss" scoped>
</style>