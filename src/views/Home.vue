<template>
<div>
  <Header/>
    <!-- Header End -->
  <About/>
    <!-- About Section End-->
  <Resume/>
    <!-- Resume Section End-->

    <section id="contact">

        <div class="row section-head">

            <div class="two columns header-col">

                <h1><span>Get In Touch.</span></h1>

            </div>

            <div class="ten columns">

                <p class="lead">Send me a notification and I promise to respond within the shortest possible time.
                </p>

            </div>

        </div>

        <div class="row">

            <div class="ten columns">
                <!-- form -->
                <form @submit.prevent="sendEmail" id="contactForm" name="contactForm">
                    <fieldset>
                        <div>
                            <label for="contactName">Name <span class="required">*</span></label>
                          <span :class="sender_name_invalid ? 'text-danger' : ''" v-if="sender_name_invalid"> {{ errors.sender_name }}</span>
                          <input required v-model="contact.sender_name" type="text" size="35" id="contactName" name="contactName">
                        </div>

                        <div>
                            <label for="contactEmail">Email <span class="required">*</span></label>
                          <span :class="sender_email_invalid ? 'text-danger' : ''" v-if="sender_email_invalid"> {{ errors.sender_email }}</span>
                          <input v-model="contact.sender_email" required type="email" id="contactEmail" name="contactEmail">
                        </div>

                        <div>
                            <label for="contactSubject">Subject</label>
                          <span :class="sender_subject_invalid ? 'text-danger' : ''" v-if="sender_subject_invalid"> {{ errors.sender_subject }}</span>
                          <input v-model="contact.sender_subject" required type="text" size="35" id="contactSubject" name="contactSubject">
                        </div>

                        <div>
                            <label for="contactMessage">Message <span class="required">*</span></label>
                          <span :class="sender_message_invalid ? 'text-danger' : ''" v-if="sender_message_invalid"> {{ errors.sender_message }}</span>
                          <textarea required v-model="contact.sender_message"  cols="50" rows="15" id="contactMessage" name="contactMessage"></textarea>
                        </div>

                        <div class="twelve columns">
                          <div v-if="response.success"><span :class="response.success ? 'text-success' : ''"
                                                             class="px-1 text-justify overflow-ellipsis"><i class="fa fa-check"></i> {{
                              response.success
                            }}</span></div>
                          <div v-if="response.error"><span :class="response.error ? 'text-danger' : ''"
                                                           class="px-1 text-justify overflow-ellipsis">{{
                              response.error
                            }}</span></div>
                        </div>

                        <div>
                            <button class="submit" type="submit" :disabled="sendMailLoading
">Submit &nbsp;</button>
                            <span v-if="sendMailLoading">
                        <img alt="" src="images/loader.gif" width="50" height="50" style="padding: 5px;">
                            </span>
                        </div>
                    </fieldset>
                </form>
                <!-- Form End -->
            </div>

        </div>

    </section>
    <!-- Contact Section End-->

  <Footer/>
    <!-- Footer End-->
</div>
</template>

<script>
import emailJs from 'emailjs-com'
// @ is an alias to /src
import Header from '@/components/Header'
import Footer from '@/components/Footer'
import About from '@/components/About'
import Resume from '@/components/Resume'

export default {
  name: 'Home',
  components: {
    Header,
    Footer,
    About,
    Resume
  },
  data () {
    return {
      contact: {
        sender_subject: '',
        sender_name: '',
        sender_email: '',
        sender_message: ''
      },
      response: {
        success: '',
        error: ''
      },
      errors: {
        sender_name: '',
        sender_subject: '',
        sender_email: '',
        sender_message: ''
      },
      sendMailLoading: false
    }
  },
  methods: {
    sendEmail () {
      this.resetErrors()
      if (this.contact.sender_name.length < 3) {
        this.errors.sender_name = 'Name is too short'
        return
      }
      if (this.contact.sender_email.length < 10) {
        this.errors.sender_email = 'Please entered a valid email'
        return
      }
      if (this.contact.sender_subject.length < 4) {
        this.errors.sender_subject = 'Subject is too short'
        return
      }
      if (this.contact.sender_message.length < 5) {
        this.errors.sender_message = 'Message is too short'
        return
      }
      const mailFormat = /^[a-zA-Z0-9]+[.-_]?[a-zA-Z0-9]*@[a-zA-Z-]+(.[a-zA-Z0-9]{2,4})/
      if (!this.contact.sender_email.match(mailFormat)) {
        this.errors.sender_email = 'The email entered is invalid.'
        return
      }
      this.sendMailLoading = true
      emailJs.send('service_gblend-tech', 'template_gblend-tech', {
        to_name: 'Gabriel',
        website_url: location.host,
        sender_name: this.contact.sender_name,
        sender_subject: this.contact.sender_subject,
        sender_email: this.contact.sender_email,
        sender_message: this.contact.sender_message
      }, 'user_5p9v05kB454yOL37NR33D')
        .then((result) => {
          this.sendMailLoading = false
          this.contact.sender_name = this.contact.sender_email = this.contact.sender_subject = this.contact.sender_message = ''
          if (result.status === 200 && result.text === 'OK') {
            this.response.success = 'Email received! I will get in touch with you as soon as possible.'
            setTimeout(() => {
              this.response.success = ''
            }, 7000)
          }
        }, () => {
          this.sendMailLoading = false
          this.response.error = 'Oops! email was not sent. Please try again later.'
          setTimeout(() => {
            this.response.error = ''
          }, 7000)
        })
    },
    resetErrors () {
      this.errors.sender_name = this.errors.sender_email = this.errors.sender_subject = this.errors.sender_message = ''
    }
  },
  computed: {
    sender_name_invalid () {
      return this.contact.sender_name.length < 3
    },
    sender_email_invalid () {
      return this.contact.sender_email.length < 10
    },
    sender_subject_invalid () {
      return this.contact.sender_subject.length < 4
    },
    sender_message_invalid () {
      return this.contact.sender_message.length < 5
    }
  },
  created () {
    this.resetErrors()
  }
}
</script>

<style scoped>
.text-danger {
  color: #F93154;
  text-align: center!important;
  margin-left: 150px;
}
.text-success, .fa-check {
  color: #00B74A;
  text-align: center!important;
  margin-left: 50px;
}
</style>
