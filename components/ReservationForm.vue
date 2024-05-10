<script setup>
import { toast } from 'vue3-toastify';
import Swal from 'sweetalert2'




let data = {
  email: ''
}
const code = ref('');
//si le compte n'existe pas lien expire
onMounted(() => {
  const route = useRoute();
  const condition = route.query.code;
  code.value = condition;
})

const soumettre = async () => {
  //check if not empty
  if (data.email === '') {
    toast.warning('Veuillez entrer votre email');
    return;
  }
  //check if email is valid with regex
  let emailRegex = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$/;
  if (!emailRegex.test(data.email)) {
    toast.warning('Veuillez entrer un email valide');
    return;
  }

  //swal loading
  Swal.fire({
    title: 'Envoi en cours...',
    allowOutsideClick: false,
    showConfirmButton: false,
    willOpen: () => {
      Swal.showLoading()
    },
  })

  //send email to backend
  try {
    const response = await useFetch(`http://localhost:8000/api/reservation/add/${code.value}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: {
        email: data.email
      }
    });
    console.log(response.data.value);
    Swal.close();
    if (response.data.value.status === 'success') {
      toast.success(response.data.value.message);

    } else if (response.data.value.status === 'warning') {
      toast.warning(response.data.value.message);
    }
    else {
      toast.error(response.data.value.message);
    }
  } catch (error) {
    toast.error('Une erreur est survenue lors de la réservation' + error.message);
  }
}

</script>
<template>

  <body class="bg-gray-100 dark:bg-gray-800">
    <section class="flex items-center justify-center h-screen">
      <div class="bg-white dark:bg-gray-900 py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6 rounded-lg shadow-md">
        <div class="mx-auto max-w-screen-md sm:text-center">
          <h2 class="mb-4 text-3xl tracking-tight font-extrabold text-gray-900 sm:text-4xl dark:text-white">Réservez
            votre place dès maintenant</h2>
          <p class="mx-auto mb-8 max-w-2xl font-light text-gray-500 md:mb-12 sm:text-xl dark:text-gray-400">Assurez-vous
            d'obtenir votre accès à l'événement en réservant dès maintenant. Nous aimerions vous accueillir et apprécier
            votre présence .</p>
          <div class="items-center mx-auto mb-3 space-y-4 max-w-screen-sm sm:flex sm:space-y-0">
            <div class="relative w-full">
              <label for="email" class="hidden mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Email
                address</label>
              <div class="flex absolute inset-y-0 left-0 items-center pl-3 pointer-events-none">
                <svg class="w-5 h-5 text-gray-500 dark:text-gray-400" fill="currentColor" viewBox="0 0 20 20"
                  xmlns="http://www.w3.org/2000/svg">
                  <path d="M2.003 5.884L10 9.882l7.997-3.998A2 2 0 0016 4H4a2 2 0 00-1.997 1.884z">
                  </path>
                  <path d="M18 8.118l-8 4-8-4V14a2 2 0 002 2h12a2 2 0 002-2V8.118z"></path>
                </svg>
              </div>
              <input
                class="block p-3 pl-10 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 sm:rounded-none sm:rounded-l-lg focus:ring-primary-500 focus:border-primary-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500"
                placeholder="Entrer votre email" v-model="data.email" type="text" id="email" required="">
            </div>
            <div>
              <button type="submit"
                class="py-3 px-5 w-full text-sm font-medium text-center text-white rounded-lg border cursor-pointer bg-primary-700 border-primary-600 sm:rounded-none sm:rounded-r-lg hover:bg-primary-800 focus:ring-4 focus:ring-primary-300 dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800"
                @click="soumettre()">Reserver</button>
            </div>
          </div>
          <div
            class="mx-auto max-w-screen-sm text-sm text-left text-gray-500 newsletter-form-footer dark:text-gray-300">
            Nous nous soucions de la protection de vos données. <a href="#"
              class="font-medium text-primary-600 dark:text-primary-500 hover:underline">Lisez notre Politique de
              confidentialité</a>.
          </div>
        </div>
      </div>
    </section>
  </body>
</template>