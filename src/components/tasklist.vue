<template>
<div>
    <h1>Your Todays Tasklist</h1>
    <div class="listtable">
        <table>
            <tr>
                <th> Id </th>
                <th> Task Name </th>
                <th> Action </th>
            </tr>
            <tr v-for="i in task" :key="i.id">
                <td>{{i.id}}</td>
                <td>{{i.taskname}}</td>
                <td>
                    <button class="tablebtn update" v-on:click="taskUpdate(i.id)">Update</button>
                    <button class="tablebtn delete" v-on:click="deletetask(i.id)">Completed</button>
                </td>
            </tr>
        </table>

        <div class="addtask" v-if="show">
            <input type="text" v-model="taskname">
            <button v-if="submitshow" class="submitbtn addtaskbtn" v-on:click="submitdata()"> Submit</button>
            <button v-if="updatebtnshow" class="submitbtn addtaskbtn" v-on:click="updateData({taskid})"> Update</button>
        </div>
        <button v-if="!show" class="addtaskbtn" v-on:click="showInput()"> Add task</button>
    </div>
</div>
</template>

<script>
import axios from 'axios'
export default {
    name: "Tasklist",
    data() {
        return {
            task: [],
            show: false,
            taskid: "",
            taskname: "",
            updatebtnshow: true,
            submitshow: true
        }
    },

    mounted() {

        this.onload();
    },
    methods: {
       async onload(){
        let getlist = await axios.get("http://localhost:3000/todotask");
        // console.log(getlist.data);
        this.task = getlist.data;
        this.$router.push({name:"Home"});
        },
        showInput() {
            this.show = true;
            this.updatebtnshow = false;
            this.submitshow = true;
        },
        async submitdata() {
            let submiteddata = await axios.post("http://localhost:3000/todotask", {
                taskname: this.taskname
            });

            this.show = !this.show;
            this.taskname = "";
            if(submiteddata.status==201){
                this.onload();
            }
            

        },

        async taskUpdate(id) {
            let getupdatedata = await axios.get(`http://localhost:3000/todotask/${id}`);
            this.taskname = getupdatedata.data.taskname;
            this.taskid = getupdatedata.data.id;
            this.show = true;
            this.submitshow = false;
            this.updatebtnshow = true;
            //  console.log(this.taskid);
            //  let updateData = await axios.put(`http://localhost:3000/todotask/${id}`,{
            //      taskname:this.taskname
            //  });

            //  console.log(updateData);
            //  if(updateData.status == 200){
            //      alert("update successful");
            //  }

        },
        async updateData(id) {
            // console.log("myid",id.taskid);
            let result = await axios.put(`http://localhost:3000/todotask/${id.taskid}`, {
                taskname: this.taskname
            });
            console.log(result);
            if (result.status == 200) {
                alert("update successful");
                this.onload();
            }

        },

        async deletetask(id){

            let result = await axios.delete(`http://localhost:3000/todotask/${id}`);
            console.log(result);

            if(result.status==200){
                
                alert("One task deleted");
                this.onload();
            }
            


        }
    },

}
</script>

<style scoped>
.listtable table {
    border-collapse: collapse;
    width: 50%;
    margin-left: auto;
    margin-right: auto;

}

.listtable th {
    padding: 12px 5px 0px 5px;
    padding-bottom: 12px;
    text-align: left;
    background-color: orange;
    color: white;

}

.listtable tr:nth-child(even) {
    background-color: #f2f2f2;

}

.listtable tr {
    text-align: left;

}

.listtable td {
    padding: 5px
}

.listtable tr:hover {
    background-color: #ddd;
}

.addtask {
    margin-top: 20px;
}

.addtask input {
    width: 300px;
    height: 40px;
    padding-left: 20px;
    margin-bottom: 20px;
    display: block;
    margin-left: auto;
    margin-right: auto;
    border: 2px solid orange;
    border-radius: 20px;
}

.tablebtn {
    width: 100px;
    padding: 5px;
    margin: 5px;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 5px;
}

.update {
    background-color: skyblue;
}

.delete {
    background-color: rgba(255, 0, 0, 0.541);
}

.addtaskbtn {
    width: 172px;
    padding: 11px;
    margin: 13px;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 20px;
    background-color: orange;
    font-size: 16px;
}

.submitbtn {
    background-color: gold;

}
</style>
