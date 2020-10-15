<template>
    <form-wizard ref="formwizard" @onComplete="onComplete" @onNextStep="nextStep" @onPreviousStep="previousStep" @onReset="reset">
        <tab-content title="About You" :selected="true">
            <div class="form-group">
                <label for="firstName">First Name</label>
                <input type="text" class="form-control" :class="hasError('firstName') ? 'is-invalid' : ''" placeholder="Enter your name" v-model="formData.firstName">
                <div v-if="hasError('firstName')" class="invalid-feedback">
                    <div class="error" v-if="!$v.formData.firstName.required">Please provide a valid first name.</div>
                </div>
            </div>
            <div class="form-group">
                <label for="lastName">Last Name</label>
                <input type="text" class="form-control" :class="hasError('lastName') ? 'is-invalid' : ''" placeholder="Enter your name" v-model="formData.lastName">
                <div v-if="hasError('lastName')" class="invalid-feedback">
                    <div class="error" v-if="!$v.formData.lastName.required">Please provide a valid last name.</div>
                </div>
            </div>
            <div class="form-group">
                <label for="email">Your Email</label>
                <input type="email" class="form-control" :class="hasError('email') ? 'is-invalid' : ''" placeholder="Enter your email" v-model="formData.email">
                <div v-if="hasError('email')" class="invalid-feedback">
                    <div class="error" v-if="!$v.formData.email.required">Email field is required</div>
                    <div class="error" v-if="!$v.formData.email.email">Should be in email format</div>
                </div>
            </div>
        </tab-content>
        <tab-content title="About your Company"> 
            <div class="form-group">
                <label for="companyName">Your Company Name</label>
                <input type="text" class="form-control" :class="hasError('companyName') ? 'is-invalid' : ''" placeholder="Enter your Company Name" v-model="formData.companyName">
                <div v-if="hasError('companyName')" class="invalid-feedback">
                    <div class="error" v-if="!$v.formData.companyName.required">Please provide a valid company name.</div>
                </div>
            </div>
            <div class="form-group">
                <label for="designation">Your Designation</label>
                <select :class="hasError('designation') ? 'is-invalid' : ''" class="form-control" v-model="formData.designation">
                    <option>Developer</option>
                    <option>Manager</option>
                </select>
                <div v-if="hasError('designation')" class="invalid-feedback">
                    <div class="error" v-if="!$v.formData.designation.required">Please select a designation.</div>
                </div>
            </div>
        </tab-content>
        <tab-content title="Finishing Up">
            <div class="form-group">
                <label for="referral">From Where did you hear about us</label>
                <select :class="hasError('referral') ? 'is-invalid' : ''" class="form-control" v-model="formData.referral">
                    <option>Newspaper</option>
                    <option>Online Ad</option>
                    <option>Friend</option>
                    <option>Other</option>
                </select>
                <div v-if="hasError('referral')" class="invalid-feedback">
                    <div class="error" v-if="!$v.formData.referral.required">Please select on of the fields.</div>
                </div>
            </div>
            <div class="form-group form-check">
                <input type="checkbox" :class="hasError('terms') ? 'is-invalid' : ''" class="form-check-input" v-model="formData.terms">
                <label class="form-check-label">I accpet terms and conditions</label>
                <div v-if="hasError('terms')" class="invalid-feedback">
                    <div class="error" v-if="!$v.formData.terms.required">Please select terms and conditions.</div>
                </div>
            </div>
        </tab-content>
    </form-wizard>
</template>

<script>
import FormWizard from '../components/FormWizard.vue';
import TabContent from '../components/TabContent.vue';
import ValidationHelper from '../components/ValidationHelper.vue';
import { required } from 'vuelidate/lib/validators';
import { email } from 'vuelidate/lib/validators';
// import { numeric } from 'vuelidate/lib/validators';
const checked = (value) => value === true;

export default {
    name: 'SimpleStepper',
    components: {
        FormWizard, TabContent
    },
    mixins: [ValidationHelper],
    data(){
        return {
            formData:{
                firstName: '',
                lastName: '',
                email: null,
                companyName: null,
                designation: null,
                referral: null,
                terms: false,
            },
            validationRules:[
                {firstName: {required}, lastName: {required}, email: {required, email} },
                {companyName: {required}, designation: {required} },
                {referral: {required}, terms: {required, checked} }
            ]
        }
    },
    methods:{
        onComplete(){
            alert("Thank you! Your form has been submitted.");
            this.$refs.formwizard.changeStatus();
        },

        reset(){
            for(let field in this.formData){
                this.formData[field] = null;
            }
        },

        nextStep(){
            //alert("On Next Step");
        },

        previousStep(){
            //alert("On Previous Step");
        }
    }
}
</script>