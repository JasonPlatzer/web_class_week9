<template>
 <div id="app">
   <!-- the new-student-form is a conversion of NewStudentForm-->
   <!-- student-added from NewStudentForm-->
   <!-- when new-student occurs newStudent is called-->
   <new-student-form v-on:student-added="newStudentAdded"></new-student-form>
   <!-- binds with students from StudentTable-->
   <student-table v-bind:students="students" v-on:student-arrived-or-left="studentArrivedOrLeft" 
   v-on:delete-student="studentDeleted">
   </student-table>
  <!-- student is prop in student message-->
   <student-message v-bind:student="mostRecentStudent"></student-message> 

 </div> 
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'

export default {
  name: 'App',
  emits:['student-added'],

  data(){
    return{
      students: [],
      mostRecentStudent:{}

    }
  },
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable
  },
  methods: {
    // student is data from NewStudentForm
      newStudentAdded(student){
      this.students.push(student)
      this.students.sort(function(s1,s2){ 
      return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1

      })
    },
    // student and present are from student table
    studentArrivedOrLeft(student, present){
      // find returns firs matching element in an array
      let updateStudent = this.students.find(function(s){
        if (s.name === student.name && s.starID === student.starID){
          return true
        }
      })
      if (updateStudent){
        updateStudent.present = present
        this.mostRecentStudent = updateStudent
      }
    },
    studentDeleted(student){
      // filter checks every student in array if student matches a condition they will be kept
      //if student doesn't match condition they will be filtered out, removed from array
      // filter returns a new array of elements that return true
      // student comes from StudentRow
      this.students = this.students.filter(function(s){
        if (s != student){
          return true
        }
      })
      this.mostRecentStudent = {}
    }
  }
}
</script>

<style>
@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css";

</style>
<!-- would you rather question: 