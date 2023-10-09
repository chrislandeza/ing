<template>
  <q-layout view="hHh lpr fff" class="gp-login-layout">
    <q-page-container class="bg-grey-2">
      <q-page class="flex flex-center">
        <q-form class="q-pa-md" @submit="handleSubmit">
          <q-card flat bordered class="q-pb-md">
            <div class="row q-my-lg">
              <div class="col-12 text-center">
                <div class="text-h2 flex items-center justify-center">
                  <img
                    alt="Quasar logo"
                    src="~assets/logo.gif"
                  >
                </div>
              </div>
            </div>
            <div class="row q-mb-lg">
              <div class="col-12 text-center">
                <div class="flex items-center justify-center text-grey-9 text-h6 text-weight-bold">
                  Register
                </div>
              </div>
            </div>
            <div class="row justify-center q-col-gutter-md q-pa-none" style="max-width: 500px">
              <q-input
                v-model="form.name"
                class="col-xs-10 col-sm-10 col-md-10"
                autofocus
                label="Name"
                outlined
                bottom-slots
                :error="v$.name.$invalid"
                @blur="v$.name.$touch()"
              >
                <template #error>
                  <ing-vuelidate-field-errors :errors="v$.name.$errors"/>
                </template>
              </q-input>

              <q-input
                v-model="form.email"
                class="col-xs-10 col-sm-10 col-md-10"
                name="email"
                label="Email"
                bottom-slots
                outlined
                :error="v$.email.$invalid"
                @blur="v$.email.$touch()"
              >
                <template #error>
                  <ing-vuelidate-field-errors :errors="v$.email.$errors"/>
                </template>
              </q-input>

              <q-list class="col-xs-10 col-sm-10 col-md-10">
                <q-item tag="label">
                  <q-item-section>
                    <q-item-label class="text-subtitle2">Are you an ING Employee?</q-item-label>
                  </q-item-section>
                  <q-item-section side>
                    <q-btn-toggle
                      v-model="form.isEmployee"
                      no-caps
                      unelevated
                      spread
                      :options="[
                        {label: 'Yes', value: true, flat: true},
                        {label: 'No', value: false, flat: true}
                      ]"
                    />
                  </q-item-section>
                </q-item>
              </q-list>

              <div class="col-xs-10 col-sm-10 col-md-8 text-center">
                <q-btn
                  type="submit"
                  label="Submit"
                  color="primary"
                  unelevated
                  no-caps
                ></q-btn>
              </div>
            </div>
          </q-card>
        </q-form>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { useQuasar } from 'quasar'
import { useVuelidate } from '@vuelidate/core'
import { required, email } from '@vuelidate/validators'

import IngVuelidateFieldErrors from 'components/IngVuelidateFieldErrors.vue'
import { useRouter } from 'vue-router'
import { ref, reactive } from 'vue'

const ING_EMAIL_DOMAINS = ['ing.com', 'ing.net']

export default {

  components: {
    IngVuelidateFieldErrors
  },
  setup () {
    const $q = useQuasar()
    const router = useRouter()
    const form = reactive(
      {
        name: '',
        email: '',
        isEmployee: true
      }
    )
    const rules = {
      name: { required, $lazy: true }, // Matches state.firstName
      email: { required, email, $lazy: true } // Matches state.contact.email
    }

    const v$ = useVuelidate(rules, form)

    const handleSubmit = () => {
      const emailDomain = extractDomain()

      if (form.isEmployee && !ING_EMAIL_DOMAINS.includes(emailDomain)) {
        $q.dialog({
          title: 'Confirmation',
          message: `The email address you entered (${form.email}) doesn't appear to be a valid ING email. Do you want to proceed?`,
          cancel: {
            noCaps: true,
            flat: true
          },
          ok: {
            label: 'Proceed',
            flat: true,
            noCaps: true
          },
          html: true,
          persistent: true
        }).onOk(() => {
          process()
        })
      }

      if (!form.isEmployee && ING_EMAIL_DOMAINS.includes(emailDomain)) {
        $q.dialog({
          title: 'Confirmation',
          message: `The email address you entered (awdwa@ing.com) appears to be associated with ING. Are you certain you are not an ING employee?`,
          cancel: {
            noCaps: true,
            flat: true
          },
          ok: {
            label: 'Yes, Proceed  ',
            flat: true,
            noCaps: true
          },
          html: true,
          persistent: true
        }).onOk(() => {
          process()
        })
      }

    }

    const process = () => {
      console.log('processing...')
    }

    const extractDomain = () => {
      const domainRegex = /@([^@]+)$/

      const match = form.email.match(domainRegex)

      return (match) ? match[1].toLowerCase() : ''
    }

    return {
      form,
      v$,
      handleSubmit
    }
  }
}
</script>


