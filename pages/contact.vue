<template>
  <div class="grid grid-cols-1 lg:grid-cols-2">
    <div>
      <TheCard>
        <WYSIWYG :content="contact.title"></WYSIWYG>
        <div v-if="!this.form.submitted">
          <input v-model="form.name" class="mb-3 sm:mb-4" type="text" name="name" placeholder="Name">
          <input v-model="form.subject" class="mb-3 sm:mb-4" type="text" name="subject" placeholder="Subject">
          <input v-model="form.replyto" class="mb-3 sm:mb-4" type="email" name="replyto" placeholder="Email">
          <textarea v-model="form.message" class="h-48 sm:h-56 resize-none" placeholder="Message" name="message"></textarea>
        </div>
        <WYSIWYG v-if="this.form.incorrect && !this.form.submitted">
          <p>Please complete all fields.</p>
        </WYSIWYG>
        <span @click="send()" v-if="!this.form.submitted">
          <PillButton w="w-80">Send</PillButton>
        </span>
        <WYSIWYG v-if="this.form.submitted">
          <p v-if="this.form.success">Success! We will get back to you shortly.</p>
          <p v-else-if="this.form.failed">Message failed to send. Please try emailing us directly at {{ contact.forward_messages_to }}.</p>
          <p v-else>Submitting...</p>
        </WYSIWYG>
      </TheCard>
    </div>
    <div>
      <TheCard nospacer="true">
        <TheImage class="rounded-xl w-full mb-5 md:mb-7" :asset="contact.banner" />
        <div class="sm:p-10 p-6 cf-card-content">
          <WYSIWYG :content="contact.social_title"></WYSIWYG>
          <div>
            <a style="color: #4267B2" :href="settings.facebook_link" v-if="settings.facebook" target="_blank" class="inline-block mr-2 sm:mr-3">
              <span class="sr-only">Facebook</span>
              <svg class="w-20 h-20" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                <path
                  fill-rule="evenodd"
                  d="M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12z"
                  clip-rule="evenodd" />
              </svg>
            </a>
            <a style="color: #C13584" :href="settings.instagram_link" v-if="settings.instagram" target="_blank" class="inline-block mr-2 sm:mr-3">
              <span class="sr-only">Instagram</span>
              <svg class="w-20 h-20" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                <path
                  fill-rule="evenodd"
                  d="M12.315 2c2.43 0 2.784.013 3.808.06 1.064.049 1.791.218 2.427.465a4.902 4.902 0 011.772 1.153 4.902 4.902 0 011.153 1.772c.247.636.416 1.363.465 2.427.048 1.067.06 1.407.06 4.123v.08c0 2.643-.012 2.987-.06 4.043-.049 1.064-.218 1.791-.465 2.427a4.902 4.902 0 01-1.153 1.772 4.902 4.902 0 01-1.772 1.153c-.636.247-1.363.416-2.427.465-1.067.048-1.407.06-4.123.06h-.08c-2.643 0-2.987-.012-4.043-.06-1.064-.049-1.791-.218-2.427-.465a4.902 4.902 0 01-1.772-1.153 4.902 4.902 0 01-1.153-1.772c-.247-.636-.416-1.363-.465-2.427-.047-1.024-.06-1.379-.06-3.808v-.63c0-2.43.013-2.784.06-3.808.049-1.064.218-1.791.465-2.427a4.902 4.902 0 011.153-1.772A4.902 4.902 0 015.45 2.525c.636-.247 1.363-.416 2.427-.465C8.901 2.013 9.256 2 11.685 2h.63zm-.081 1.802h-.468c-2.456 0-2.784.011-3.807.058-.975.045-1.504.207-1.857.344-.467.182-.8.398-1.15.748-.35.35-.566.683-.748 1.15-.137.353-.3.882-.344 1.857-.047 1.023-.058 1.351-.058 3.807v.468c0 2.456.011 2.784.058 3.807.045.975.207 1.504.344 1.857.182.466.399.8.748 1.15.35.35.683.566 1.15.748.353.137.882.3 1.857.344 1.054.048 1.37.058 4.041.058h.08c2.597 0 2.917-.01 3.96-.058.976-.045 1.505-.207 1.858-.344.466-.182.8-.398 1.15-.748.35-.35.566-.683.748-1.15.137-.353.3-.882.344-1.857.048-1.055.058-1.37.058-4.041v-.08c0-2.597-.01-2.917-.058-3.96-.045-.976-.207-1.505-.344-1.858a3.097 3.097 0 00-.748-1.15 3.098 3.098 0 00-1.15-.748c-.353-.137-.882-.3-1.857-.344-1.023-.047-1.351-.058-3.807-.058zM12 6.865a5.135 5.135 0 110 10.27 5.135 5.135 0 010-10.27zm0 1.802a3.333 3.333 0 100 6.666 3.333 3.333 0 000-6.666zm5.338-3.205a1.2 1.2 0 110 2.4 1.2 1.2 0 010-2.4z"
                  clip-rule="evenodd" />
              </svg>
            </a>
            <a style="color: #1DA1F2" :href="settings.twitter_link" v-if="settings.twitter" target="_blank" class="inline-block">
              <span class="sr-only">Twitter</span>
              <svg class="w-20 h-20" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                <path
                  d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84" />
              </svg>
            </a>
          </div>
        </div>
      </TheCard>
    </div>
  </div>
</template>

<script>
  export default {
    async fetch() {
      this.contact = await this.$api('Contact')
      this.settings = await this.$api('Settings')
    },
    data: () => ({
      contact: {},
      settings: {},
      form: {
        name: '',
        subject: '',
        replyto: '',
        message: '',
        submitted: false,
        failed: false,
        success: false,
        incorrect: false
      }
    }),
    methods: {
      send() {
        if (this.form.name && this.form.subject && this.form.replyto && this.form.replyto.match(/(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])/) && this.form.message) {
          let url = 'https://script.google.com/macros/s/AKfycbxy_UE7lKhOXSBGz2q8jZ-yIcP2XLNSNB_7LD6Fb-LTBtlJtpwEBvLj6EJFsb85dvSh/exec'
          this.form.submitted = true
          fetch(`${url}?name=${this.form.name}&subject=${this.form.subject}&replyto=${this.form.replyto}&message=${this.form.message}`).then(res => res.text()).then(text => {
            if (text === 'success') {
              this.form.success = true
            } else {
              this.form.failed = true
            }
          })
        } else {
          this.form.incorrect = true
        }
      }
    },
    head() {
      return {
        title: 'Contact - CancerFIT'
      }
    }
  }
</script>