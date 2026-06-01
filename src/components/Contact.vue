<script setup>
    import { Notyf } from 'notyf';
    import { ref, onMounted, onBeforeUnmount } from 'vue';

    const notyf = new Notyf();

    const name = ref('');
    const email = ref('');
    const message = ref('');
    const isLoading = ref(false);

    // Web3Forms Access Key used to authenticate form submissions.
	const WEB3FORMS_ACCESS_KEY = "73264586-b11b-4fb7-8ceb-b7db13b6b115";

    // Email subject that will appear when a form submission is received.
	const subject = "New message from Portfolio Contact Form";

    // The submitForm() function handles the contact form submission.
	const submitForm = async () => {

		// Ensure the user completes the reCAPTCHA challenge before submitting the form.
        // Check if a reCAPTCHA token exists
        // recaptchaToken.value - stores the verification token returned by Google reCAPTCHA
		// if(!recaptchaToken.value) {
		// 	notyf.error('Please verify that you are not a robot');
		// 	// Stop the form submission process
		// 	return;
		// }

		// While the email is being sent, disable the button and change it text to "Sending..."
		isLoading.value = true;

		try {

			// fetch() API is a built-in JavaScript function used to send HTTP requests to a server.
			const response = await fetch("https://api.web3forms.com/submit", {
				method: "POST",
				headers: {
					"Content-Type": "application/json",
					// Indicates that the request accepts a JSON response.
					Accept: "application/json"
				},
				// Convert the form data into a JSON string and include the access key.
				body: JSON.stringify({
					access_key: WEB3FORMS_ACCESS_KEY,
					subject: subject,
					name: name.value,
					email: email.value,
					message: message.value
				})
			})

			// Convert the API response into a JS Object
			const result = await response.json();

				// Check if the form submission was successful.
				if (result.success) {
					console.log(result);
					isLoading.value = false;
					notyf.success("Message Sent!");
				}
		} catch (error) {
			console.log(error);
			isLoading.value = false;
			notyf.error("Failed to send message.");
		} finally {
			// Reset the reCAPTCHA widget after the submission process completes, whether the request succeeds or fails.
			// resetRecaptcha();
		}

	}


</script>

<template>
    <!-- Start of Contact -->
    <h1 class="text-center my-4 pt-5" id="contact">Contact</h1>
    <div class="contact-section">
        <div class="row align-items-center mt-4">
            <div class="col-md-6 map-container">
                <iframe id="gmap_canvas" src="https://maps.google.com/maps?q=centro%20escolar%20university%20manila&t=&z=13&ie=UTF8&iwloc=&output=embed" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe>
            </div>
            <div class="col-md-6">
                <form @submit.prevent="submitForm">
                    <div class="mb-3">
                        <input type="text" class="form-control contact-form-control" v-model="name" placeholder="First Name M.I. Last Name">
                    </div>
                    <div class="mb-3">
                        <input type="email" class="form-control contact-form-control" v-model="email" placeholder="Email">
                    </div>
                    <div class="mb-3">
                        <textarea class="form-control contact-form-control" v-model="message" rows="6" placeholder="Message"></textarea>
                    </div>
                    <div class="form-footer">
                        <div class="social-icons">
                            <a href="https://www.linkedin.com/in/charles-babbage-8291a6211/" id="linkedin"><i class="fab fa-linkedin"></i></a>
                            <a href="https://gitlab.com/cbabbage0991" id="gitlab"><i class="fab fa-gitlab"></i></a>
                            <a href="https://github.com/cbabbage0991" id="github"><i class="fab fa-github"></i></a>
                        </div>
                        <button type="submit" class="submit-btn pl-5 pr-5" :disabled="isLoading">{{ isLoading ? 'Sending...' : 'Submit' }}</button>
                    </div>
                </form>
                
            </div>
        </div>
    </div>
    <!-- End of Contact -->
</template>

<style scoped></style>