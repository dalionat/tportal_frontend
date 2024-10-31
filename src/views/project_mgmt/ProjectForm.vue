<template>
        <form @submit.prevent="submitForm">
          <div class="columns">
            <div class="column is-8">
        
          <div class="field">
              <label for="title">عنوان پروژه</label>
              <div class="control">
                  <input type="text" name="title" class="input" v-model="title">
              </div>
          </div>
          </div>
          <div class="column is-4">
            <div class="field">
              <label for="department">اداره متولی</label>
                  <div class="control">
                      <DepartmentSelect @change="departmentChanged($event)" ref="DepartmentSelect" v-model="department"> </DepartmentSelect>
                  </div>  
          </div>
          </div>
          </div>
          <div class="field">
              <label for="description">شرح</label>
                  <div class="control">
                      <textarea class="textarea" name="description" placeholder="شرح پروژه" v-model="description"></textarea>
                  </div>
          </div>
          <div class="columns">
            <div class="column is-4"><div class="field">
              <label for="description">مدیر پروژه</label>
                  <div class="control">
                      <!-- <input type="text" name="department" class="input" v-model="department"> -->
                       <ManagerSelect @change="managerChanged($event)" ref="ManagerSelect" v-model="manager"/>
                  </div>
          </div></div>
            <div class="column is-4"><div class="field">
              <label for="description">وضعیت</label>
                  <div class="control">
                      <StatusSelect @change="statusChanged($event)" ref="StatusSelect" />
                  </div>
          </div></div>
            <div class="column is-4"><div class="field">
              <label for="description">نوع پروژه</label>
                  <div class="control">
                      <TypeSelect @change="typeChanged($event)" ref="TypeSelect" />

                  </div>
          </div></div>
          </div>
          <div class="columns">
            <div class="column is-4"><div class="field">
                <label for="description">تقویم</label>
                  <div class="control">
                      <!-- <input type="text" name="department" class="input" v-model="department"> -->
                      <CalendarSelect @change="calendarChanged($event)" ref="CalendarSelect" v-model="calendar"/>
                  </div>
              
           </div></div>
            <div class="column is-4"><div class="field">
              <label for="description">تاریخ شروع</label>
                  <div class="control">
                      <!-- <input type="text" name="department" class="input" v-model="department"> -->
                       <JDatePicker ref="JDatePicker" v-model="start_date" />
                  </div>
          </div></div>
            
            <div class="column is-4"><div class="field">
              <label for="description">تاریخ پایان</label>
                  <div class="control">
                      <!-- <input type="text" name="department" class="input" v-model="department"> -->
                      <JDatePicker ref="JDatePicker" v-model="end_date"/>
                  </div>
          </div></div>
         
          </div>
          <p v-if="errors.length">
                <b>لطفا موارد زیر را تصحیح  نمایید</b>
                <ul>
                <li class="has-text-danger" v-for="error in errors">{{ error }}</li>
                </ul>
          </p>
        </form>
</template>

<script>
import JDatePicker from '@/components/JDatePicker.vue';
import ManagerSelect from '@/components/ManagerSelect.vue';
import StatusSelect from '@/components/StatusSelect.vue';
import TypeSelect from '@/components/TypeSelect.vue';
import DepartmentSelect from '@/components/DepartmentSelect.vue';
import CalendarSelect from '@/components/CalendarSelect.vue';
import moment from 'jalali-moment';
import axios from 'axios';

  export default {
    name: 'ProjectForm',
    data() {
        return {
            title: '',
            description: '',
            department: '',
            start_date: '',
            end_date: '',
            manager: '',
            status : '',
            project_type: '',
            calendar: '', 
            errors:[]
        }
    },
    methods: {
        submitForm(event, mode) {
        let res = this.checkForm(event);
        if (res) {
            moment.locale('en');
        const data = {
            title : this.title,
            description : this.description,
            department : this.department,
            start_date : moment.from(this.start_date, 'fa', 'YYYY/MM/DD').format('YYYY-MM-DD'),
            end_date : moment.from(this.end_date, 'fa', 'YYYY/MM/DD').format('YYYY-MM-DD'),
            manager : this.manager,
            status : this.status,
            project_type : this.project_type,
            calendar : this.calendar
        }
        axios.post('/api/project_mgmt/projects', data)
            .then(response => {
                console.log('Sakhtem');
                this.$router.go(0);
            })
            .catch(error => {
                if(error.response) {
                    // for(const prop in error.response.data) {
                    //     this.errors.push(`${prop} : ${error.response.data[prop]}`)
                    // }
                    console.log(JSON.stringify(error.response.data))
                }
                else if (error.message){
                    console.log(JSON.stringify(error.message));
                }
                else {
                    console.log(JSON.stringify(error));
                    
                }
            });
        } else {
            return false;
        }
        
        },
        statusChanged(event) {
            this.status = event.target.value;
            
        },
        managerChanged(event) {
            this.manager = event.target.value;
            console.log(this.manager);
        },
        typeChanged(event) {
            this.project_type = event.target.value;
            console.log(this.type);
        },
        departmentChanged(event) {
            this.department = event.target.value;
            console.log(this.type);
        },
        calendarChanged(event) {
            this.calendar = event.target.value;
            console.log(this.calendar);
        }, 

        checkForm(event) {
            this.errors = [];
            let flag = true;
            if (!this.title) {
                this.errors.push('عنوان پروژه الزامی است')
                flag = false
            }
            if (!this.description) {
                this.errors.push('شرح پروزه الزامی است')
                flag = false
            }
            if (!this.department) {
                this.errors.push('لطفا اداره متولی را انتخاب نمایید')
                flag = false
            }
            if (!this.start_date) {
                this.errors.push('تاریخ شروع الزامی است')
                flag = false
            }
            if (!this.end_date) {
                this.errors.push('تاریخ پایان الزامی است')
                flag = false
            }
            if (!this.manager) {
                this.errors.push('لطفا مدیر پروژه را انتخاب نمایید')
                flag = false
            }
            if (!this.status) {
                this.errors.push('لطفا وضعیت پروژه را انتخاب نمایید')
                flag = false
            }
            if (!this.project_type) {
                this.errors.push('لطفا نوع پروژه را مشخص کنید')
                flag = false
            }
            if (!this.calendar) {
                this.errors.push('لطفا تقویم پروژه را انتخاب نمایید')
                flag = false
            }
            if (!flag) {
               return false;
            } else {
                return true;
            }
            

        }
        
    },
    components : {
        JDatePicker,
        ManagerSelect,
        StatusSelect,
        TypeSelect,
        DepartmentSelect,
        CalendarSelect
    }
    
  };
</script>
<style>
.form-control .textarea {
    font-family: 'Vazir' ;
}

</style>