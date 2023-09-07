<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                <h4>
                    Students
                    <RouterLink to="/students/create" class="btn btn-primary float-end">
                        Add Student
                    </RouterLink>
                </h4>
            </div>
            <div class="card-body">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>id</th>
                            <th>name</th>
                            <th>course</th>
                            <th>email</th>
                            <th>phone</th>
                            <th>created_at</th>
                            <th>updated_at</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody v-if="students.length > 0">
                        <tr v-for="(student, index) in students" :key="index">
                            <td>{{ student.id }}</td>
                            <td>{{ student.name }}</td>
                            <td>{{ student.course }}</td>
                            <td>{{ student.email }}</td>
                            <td>{{ student.phone }}</td>
                            <td>{{ student.created_at }}</td>
                            <td>{{ student.updated_at }}</td>
                            <td>
                                <div class="btn-group">
                                    <RouterLink :to="{ path: '/students/'+student.id+'/edit'}"  class="btn btn-success mx-2"> <!-- Add margin on the right -->
                                    Edit
                                    </RouterLink>
                                    <button type="button" @click="deleteStudent(student.id)" class="btn btn-danger">
                                    Delete
                                    </button>
                                </div>
                             </td>
                        </tr>
                    </tbody>
                    <tbody v-else>
                        <tr>
                            <td colspan="8">Loading...</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default{
    name: 'students',
    data(){
        return {
            students:[]
        }
    },
    mounted(){
        this.getStudents();
        // console.log('i am here');
    },
    methods:{
        getStudents(){
            axios.get('http://localhost:8000/api/students').then( res=>{
                this.students = res.data.students
                console.log(this.students);
            });
        },

        deleteStudent(studentId){
            if(confirm('Are you sure, you want to delete this data?')){
                // console.log(studentId);
                axios.delete(`http://localhost:8000/api/students/${studentId}/delete`)
                .then(res => {
                   alert(res.data.message);
                   this.getStudents(); 
                })
                .catch(function (error) {
                        if (error.response) {
                            if (error.response.status == 422) {
                                mythis.errorList = error.response.data.errors;
                            }
                            if (error.response.status == 404) {
                                alert(error.response.data.message);
                            }
                            /* console.log(error.response.data);
                            console.log(error.response.status);
                            console.log(error.response.headers); */
                        } else if (error.request) {
                            console.log(error.request);
                        } else {
                            console.log('Error', error.message);
                        }
                    });
            }
            console.log(studentId);
        }
    },
}
</script>
