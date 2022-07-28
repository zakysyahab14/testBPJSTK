<template>
  <div id="form-detail">
    <v-form
      ref="form"
      v-model="detailFormAll"
      lazy-validation
    >
      <!-- personal details  -->
      <div class="personal-details">
        <p class="title-step mb-5">
          Personal Details
        </p>
        <div class="row">
          <v-text-field
            v-model="form.jobTitle"
            filled
            label="Wanted Job Title"
            class="col-6 form-gap"
          />
          <v-file-input
            v-model="form.photoCv"
            :rules="imageRules"
            filled
            accept="image/png, image/jpeg, image/bmp"
            placeholder="Pick an avatar"
            prepend-icon="mdi-account"
            class="col-6 form-gap"
            label="Upload Photo"
          />
          <v-text-field
            v-model="form.firstName"
            filled
            label="First Name"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.lastName"
            filled
            label="Last Name"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.email"
            filled
            :rules="emailRules"
            label="Email"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.phone"
            filled
            :rules="phoneRules"
            label="Phone"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.country"
            filled
            label="Country"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.city"
            filled
            label="City"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.address"
            filled
            label="Address"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.postalCode"
            filled
            label="Postal Code"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.drivingLicense"
            filled
            label="Driving License"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.nationality"
            filled
            label="Nationality"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.placeOfBirth"
            filled
            label="Place Of Birth"
            class="col-6 form-gap"
          />
          <v-text-field
            v-model="form.dateOfBirth"
            filled
            label="Date Of Birth"
            class="col-6 form-gap"
          />
        </div>
      </div>
      <!-- profesional summary -->
      <div class="profesional-summary">
        <p class="title-step">
          Professional Summary
        </p>
        <div class="description-part">
          Write 2-4 short & energetic sentences to interest the reader! Mention your role, experience & mist importantly - your biggest achievements, best qualities and skills.
        </div>
        <wysiwyg v-model="description" />
      </div>
      <!-- employment history -->
      <div class="employment-history">
        <p class="title-step">
          Employment History
        </p>
        <div class="description-part">
          Show your relevant experience (last 10years). Use bullet points to note your achievements. If possible - use numbers/facts (Achieved X, measure by Y, by doing Z)
        </div>
        <div v-for="(employ, index) in employmentHistory" :key="index" class="card-employe">
          <p class="title-card">
            {{ employ.jobTitleEH }} at {{ employ.employerEH }}
          </p>
          <p class="desc-card">
            {{ employ.startDateEH }} - {{ employ.endDateEH }}
          </p>
        </div>
        <v-dialog
          v-model="dialog"
          max-width="800px"
        >
          <template #activator="{ on, attrs }">
            <div
              color="primary"
              dark
              class="mb-2 add-more"
              v-bind="attrs"
              v-on="on"
            >
              + Add one more employment
            </div>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">Employment history</span>
            </v-card-title>

            <v-card-text>
              <v-container class="row">
                <v-text-field
                  v-model="editedItem.jobTitleEH"
                  filled
                  label="Job Title"
                  class="col-6 form-gap"
                />
                <v-text-field
                  v-model="editedItem.employerEH"
                  filled
                  label="Employer"
                  class="col-6 form-gap"
                />
                <v-menu
                  ref="menuStart"
                  v-model="menuStart"
                  :close-on-content-click="false"
                  :return-value.sync="editedItem.startDateEH"
                  transition="scale-transition"
                  offset-y
                  max-width="290px"
                  class="col-3 form-gap"
                  min-width="auto"
                >
                  <template #activator="{ on, attrs }">
                    <v-text-field
                      v-model="editedItem.startDateEH"
                      label="Start Date"
                      prepend-icon="mdi-calendar"
                      readonly
                      class="col-3 form-gap"
                      filled
                      v-bind="attrs"
                      v-on="on"
                    />
                  </template>
                  <v-date-picker
                    v-model="editedItem.startDateEH"
                    type="month"
                    no-title
                    scrollable
                  >
                    <v-spacer />
                    <v-btn
                      text
                      color="primary"
                      @click="menuStart = false"
                    >
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.menuStart.save(editedItem.startDateEH)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-menu>

                <v-menu
                  ref="menuEnd"
                  v-model="menuEnd"
                  :close-on-content-click="false"
                  :return-value.sync="editedItem.endDateEH"
                  transition="scale-transition"
                  offset-y
                  max-width="290px"
                  class="col-3 form-gap"
                  min-width="auto"
                >
                  <template #activator="{ on, attrs }">
                    <v-text-field
                      v-model="editedItem.endDateEH"
                      label="End Date"
                      readonly
                      class="col-3 form-gap"
                      filled
                      v-bind="attrs"
                      v-on="on"
                    />
                  </template>
                  <v-date-picker
                    v-model="editedItem.endDateEH"
                    type="month"
                    no-title
                    scrollable
                  >
                    <v-spacer />
                    <v-btn
                      text
                      color="primary"
                      @click="menuEnd = false"
                    >
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.menuEnd.save(editedItem.endDateEH)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-menu>
                <v-text-field
                  v-model="editedItem.cityEH"
                  filled
                  label="City"
                  class="col-6 form-gap"
                />
                <wysiwyg v-model="editedItem.descriptionEH" />
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer />
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>

      <!-- Education -->
      <div class="education">
        <p class="title-step">
          Education
        </p>
        <div class="description-part">
          A varied education on your resume sums up the value that your learnings and bacground will bring to job.
        </div>
        <div v-for="(educate, index) in educationHistory" :key="index" class="card-employe">
          <p class="title-card">
            {{ educate.degreeEH }} at {{ educate.schoolEH }}
          </p>
          <p class="desc-card">
            {{ educate.startDateEH }} - {{ educate.endDateEH }}
          </p>
        </div>
        <v-dialog
          v-model="dialogEducation"
          max-width="800px"
        >
          <template #activator="{ on, attrs }">
            <div
              color="primary"
              dark
              class="mb-2 add-more"
              v-bind="attrs"
              v-on="on"
            >
              + Add one more education
            </div>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">Education history</span>
            </v-card-title>

            <v-card-text>
              <v-container class="row">
                <v-text-field
                  v-model="editedEducation.schoolEH"
                  filled
                  label="School"
                  class="col-6 form-gap"
                />
                <v-text-field
                  v-model="editedEducation.degreeEH"
                  filled
                  label="Degree"
                  class="col-6 form-gap"
                />
                <v-menu
                  ref="menuStartEducation"
                  v-model="menuStartEducation"
                  :close-on-content-click="false"
                  :return-value.sync="editedEducation.startDateEH"
                  transition="scale-transition"
                  offset-y
                  max-width="290px"
                  class="col-3 form-gap"
                  min-width="auto"
                >
                  <template #activator="{ on, attrs }">
                    <v-text-field
                      v-model="editedEducation.startDateEH"
                      label="Start Date"
                      prepend-icon="mdi-calendar"
                      readonly
                      class="col-3 form-gap"
                      filled
                      v-bind="attrs"
                      v-on="on"
                    />
                  </template>
                  <v-date-picker
                    v-model="editedEducation.startDateEH"
                    type="month"
                    no-title
                    scrollable
                  >
                    <v-spacer />
                    <v-btn
                      text
                      color="primary"
                      @click="menuStartEducation = false"
                    >
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.menuStartEducation.save(editedEducation.startDateEH)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-menu>

                <v-menu
                  ref="menuEndEducation"
                  v-model="menuEndEducation"
                  :close-on-content-click="false"
                  :return-value.sync="editedEducation.endDateEH"
                  transition="scale-transition"
                  offset-y
                  max-width="290px"
                  class="col-3 form-gap"
                  min-width="auto"
                >
                  <template #activator="{ on, attrs }">
                    <v-text-field
                      v-model="editedEducation.endDateEH"
                      label="End Date"
                      readonly
                      class="col-3 form-gap"
                      filled
                      v-bind="attrs"
                      v-on="on"
                    />
                  </template>
                  <v-date-picker
                    v-model="editedEducation.endDateEH"
                    type="month"
                    no-title
                    scrollable
                  >
                    <v-spacer />
                    <v-btn
                      text
                      color="primary"
                      @click="menuEndEducation = false"
                    >
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.menuEndEducation.save(editedEducation.endDateEH)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-menu>
                <v-text-field
                  v-model="editedEducation.cityEH"
                  filled
                  label="City"
                  required
                  class="col-6 form-gap"
                />
                <wysiwyg v-model="editedEducation.descriptionEH" />
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer />
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="saveEducation"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>

      <!-- Skills -->
      <div class="skills-section">
        <p class="title-step">
          Skills
        </p>
        <div class="description-part">
          Choose 5 of the most important skills to show your talends! Make sure they match the keywords of the job listing if applying via an online system.
        </div>
        <div>
          <v-switch
            v-model="switchLevel"
            label="Don't show experience level"
          />
        </div>
        <div>
          <v-chip-group
            v-model="checkedSkill"
            column
            multiple
          >
            <v-chip
              v-for="(skill, index) in listSkill"
              :key="index"
              filter
            >
              {{ skill }}
            </v-chip>
          </v-chip-group>
        </div>
        <div v-for="(checked, index) in checkedSkill" :key="index" class="card-employe">
          {{ listSkill[checked] }}
          <div>
            {{ switchLevel ? 'Experienced' : '' }}
          </div>
        </div>
        <div v-if="addSkill.length">
          <div v-for="(skill, index) in addSkill" :key="index" class="card-employe">
            {{ skill.skillName }}
            <div>
              {{ switchLevel ? 'Experienced' : '' }}
            </div>
          </div>
        </div>
        <div>
          <v-dialog
            v-model="dialogSkill"
            max-width="800px"
          >
            <template #activator="{ on, attrs }">
              <div
                color="primary"
                dark
                class="mb-2 add-more"
                v-bind="attrs"
                v-on="on"
              >
                + Add one more skill
              </div>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">Add more skill</span>
              </v-card-title>
              <v-card-text>
                <v-container class="row">
                  <v-text-field
                    v-model="editedSkill.skillName"
                    filled
                    label="Skill"
                    required
                    class="col-6 form-gap"
                  />
                  <v-rating
                    v-model="editedSkill.rating"
                    background-color="grey lighten-2"
                    class="col-6 form-gap"
                    color="primary"
                    empty-icon="mdi-star-outline"
                    full-icon="mdi-star"
                    hover
                    length="5"
                    size="100"
                    value="4"
                  />
                </v-container>
              </v-card-text>
              <v-card-actions>
                <v-spacer />
                <v-btn
                  color="blue darken-1"
                  text
                  @click="close"
                >
                  Cancel
                </v-btn>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="saveSkill"
                >
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </div>
      </div>
    </v-form>
    <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="submit"
    >
      submit
    </v-btn>
  </div>
</template>
<script>
export default {
  name: 'DetailForm',
  data: () => ({
    dialog: false,
    dialogEducation: false,
    dialogSkill: false,
    detailFormAll: true,
    form: {
      jobTitle: '',
      photoCv: '',
      firstName: '',
      lastName: '',
      email: '',
      phone: '',
      country: '',
      city: '',
      address: '',
      postalCode: '',
      drivingLicense: '',
      nationality: '',
      placeOfBirth: '',
      dateOfBirth: ''
    },
    description: '',
    checkedSkill: [1, 5],
    valid: true,
    switchLevel: false,
    employmentHistory: [],
    educationHistory: [],
    listSkill: [],
    addSkill: [],
    menuEndEducation: false,
    menuEnd: false,
    menuStart: false,
    menuStartEducation: false,
    editedItem: {
      jobTitleEH: '',
      employerEH: '',
      startDateEH: '',
      endDateEH: '',
      cityEH: '',
      descriptionEH: ''
    },
    editedEducation: {
      schoolEH: '',
      degreeEH: '',
      startDateEH: '',
      endDateEH: '',
      cityEH: '',
      descriptionEH: ''
    },
    editedSkill: {
      skillName: '',
      rating: 3
    },
    editedIndex: -1,
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /.+@.+\..+/.test(v) || 'E-mail must be valid'
    ],
    phoneRules: [
      v => !!v || 'Phone is required',
      v => (v && v.length <= 13) || 'Phone number is not more than 13 number',
      v => /^[0-9]+$/.test(v) || 'Phone just contain number',
      v => (v && v.length >= 10) || 'Phone number is not less than 10 number'
    ],
    imageRules: [
      v => !!v || 'Image is required',
      v => (v.size < 2000000) || 'Image size should be less than 2 MB!'
    ]
  }),
  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogEducation (val) {
      val || this.close()
    },
    dialogSkill (val) {
      val || this.close()
    }
  },
  created () {
    this.initialize()
  },
  methods: {
    initialize () {
      this.employmentHistory = [
        {
          jobTitleEH: 'Frontend Dev',
          employerEH: 'PT BLA BLA',
          startDateEH: '2019-10',
          endDateEH: '2020-10',
          cityEH: 'Jakarta',
          descriptionEH: ''
        }
      ]
      this.educationHistory = [
        {
          schoolEH: 'Scholl A',
          degreeEH: 'S1',
          startDateEH: '2015-10',
          endDateEH: '2020-10',
          cityEH: 'Jakarta',
          descriptionEH: ''
        }
      ]
      this.addSkill = [
      ]
      this.listSkill = [
        'Java',
        'JavaScript',
        'Pyhton',
        'Vue',
        'Nuxt',
        'HTML',
        'Scss',
        'PHP',
        'C++',
        'MongoDB'
      ]
    },
    generateData () {
      const formData = new FormData()
      formData.append('job_title', this.form.jobTitle)
      formData.append('image', this.form.photoCv)
      formData.append('first_name', this.form.firstName)
      formData.append('last_name', this.form.lastName)
      formData.append('email', this.form.email)
      formData.append('phone', this.form.phone)
      formData.append('employment_history', this.employmentHistory)
      // dan seterusnya
      return formData
    },
    submit () {
      if (this.$refs.form.validate()) {
        // alert('CV sukses dibuat')
        this.$axios
          .$post('https://testbpjs.in/api/formcv', this.generateData())
          .then((response) => {
            const result = response.data
            alert(result.message)
          })
          .catch((error) => {
            console.log(error.response.data)
          })
      } else {
        this.$refs.form.validate()
      }
    },
    close () {
      this.dialog = false
      this.dialogEducation = false
      this.dialogSkill = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
      this.$nextTick(() => {
        this.editedEducation = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
      this.$nextTick(() => {
        this.editedSkill = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },
    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.employmentHistory[this.editedIndex], this.editedItem)
      } else {
        this.employmentHistory.push(this.editedItem)
      }
      this.close()
    },
    saveEducation () {
      if (this.editedIndex > -1) {
        Object.assign(this.educationHistory[this.editedIndex], this.editedEducation)
      } else {
        this.educationHistory.push(this.editedEducation)
      }
      this.close()
    },
    saveSkill () {
      if (this.editedIndex > -1) {
        Object.assign(this.addSkill[this.editedIndex], this.editedSkill)
      } else {
        this.addSkill.push(this.editedSkill)
      }
      this.close()
    }
  }
}
</script>
<style lang="scss">
#form-detail {
    .title-step {
        font-weight: 700;
        font-size: 26px;
        margin: 10px 0 0;
    }
}
    .form-gap {
        padding: 0 10px !important;
    }
    .description-part {
        color: rgb(176, 176, 176);
        margin: 0 0 20px 0;
    }
    .card-employe {
        border: 1px solid lightgray;
        padding: 15px;
        margin-bottom: 5px;
        .title-card {
            font-weight: 700;
            margin: 0;
        }
        .desc-card {
            color: rgb(176, 176, 176);
            margin: 0;
        }
    }
    .add-more {
        color: rgb(79, 146, 247);
        font-weight: 700;
        margin: 20px 0;
    }
</style>
