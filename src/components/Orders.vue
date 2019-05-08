  <template>
    <div class="container">
        <el-button @click="openModal()" class="btn-add" type="primary">Add order</el-button>
        <el-dialog :visible.sync="dialogVisible" highlight-current-row title="Add order" width="30%">
            <el-input name="name" placeholder="Please name" v-model="name"></el-input>
            <el-input placeholder="Please price" type="number" v-model="price"></el-input>
            <el-input placeholder="Please notes" type="textarea" v-model="notes"></el-input>
            <span class="dialog-footer" slot="footer">
                <el-button @click="dialogVisible = false">Cancel</el-button>
                <el-button @click="addOrder()" type="primary">Confirm</el-button>
            </span>
        </el-dialog>

        <!-- update -->
        <el-dialog :visible.sync="dialogUpdate" highlight-current-row title="Update order" width="30%">
            <el-input name="name" placeholder="Please name" v-model="name"></el-input>
            <el-input placeholder="Please price" type="number" v-model="price"></el-input>
            <el-input placeholder="Please notes" type="textarea" v-model="notes"></el-input>
            <span class="dialog-footer" slot="footer">
                <el-button @click="dialogUpdate = false">Cancel</el-button>
                <el-button @click="doneUpdate(orders)" type="primary">Confirm</el-button>
            </span>
        </el-dialog>

        <el-table :data="orders">
            <el-table-column type="index" width="50"></el-table-column>
            <el-table-column label="Name" prop="name" width="200"></el-table-column>
            <el-table-column label="Price" prop="price" width="100"></el-table-column>
            <el-table-column label="Notes" prop="notes" width="250"></el-table-column>
            <el-table-column label="Operations" width="120">
                <template slot-scope="scope">
                    <el-button @click="editOrder(scope.$index, scope.row)" size="small" type="text">Edit</el-button>
                    <el-button @click="removeOrder(scope)" size="small" type="text">Remove</el-button>
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>

  <script>
const STORAGE_KEY = "todo-storage";
import mockData from "../mockData.json";
export default {
    name: "Orders",
    data() {
        return {
            dialogVisible: false,
            dialogUpdate: false,
            name: "",
            price: "",
            notes: "",
            orders: [],
            index: 0
        };
    },
    created() {
        console.log("mock", mockData);
        localStorage.setItem(STORAGE_KEY, JSON.stringify(mockData.orders));
        this.orders = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
    },
    methods: {
        openModal() {
            this.dialogVisible = true;
            this.name = "";
            this.price = "";
            this.notes = "";
        },
        addOrder() {
            this.dialogVisible = false;
            let params = {
                id: this.orders.length,
                name: this.name,
                price: this.price,
                notes: this.notes
            };
            this.orders.push(params);
            localStorage.setItem(STORAGE_KEY, JSON.stringify(this.orders));
        },
        editOrder(index, data) {
            this.dialogUpdate = true;
            this.name = data.name;
            this.price = data.price;
            this.notes = data.notes;
            this.index = index;
        },
        doneUpdate() {
            this.dialogUpdate = false;
            this.orders[this.index].name = this.name;
            this.orders[this.index].price = this.price;
            this.orders[this.index].notes = this.notes;
            localStorage.setItem(STORAGE_KEY, JSON.stringify(this.orders));
        },
        removeOrder(data) {
            this.orders.splice(data.$index, 1);
            localStorage.setItem(STORAGE_KEY, JSON.stringify(this.orders));
        }
    }
};
</script>
<style>
.container {
    display: flex;
    flex-direction: column;
}
.container .el-table {
    max-width: 720px;
}
.container .button-add {
    width: 100px;
}
.container .btn-add {
    width: 100px;
}
.container .el-dialog__wrapper .el-input {
    margin-bottom: 10px;
}
</style>
