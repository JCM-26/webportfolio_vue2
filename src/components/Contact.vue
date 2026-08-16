<template>
    <section id="contact" class="py-5">
        <div class="container py-4">
            <div class="text-center mb-5">
                <h2 class="section-title center d-inline-block">Get In Touch</h2>
                <p class="text-muted">Have a project in mind or want to talk tech? Drop a line.</p>
            </div>

            <div class="row g-5 justify-content-center">
                <!-- Info Columns -->
                <div class="col-lg-5">
                    <div class="d-flex align-items-center mb-4">
                        <div class="contact-icon-box me-3"><i class="bi bi-envelope-fill"></i></div>
                        <div>
                            <h6 class="fw-bold mb-0">Email Me</h6>
                            <p class="text-muted mb-0">juliusceasar.magabo26@gmail.com</p>
                        </div>
                    </div>
                    <div class="d-flex align-items-center mb-4">
                        <div class="contact-icon-box me-3"><i class="bi bi-geo-alt-fill"></i></div>
                        <div>
                            <h6 class="fw-bold mb-0">Location</h6>
                            <p class="text-muted mb-0">North Caloocan city, Philippines</p>
                        </div>
                    </div>
                    <div class="d-flex align-items-center">
                        <div class="contact-icon-box me-3"><i class="bi bi-linkedin"></i></div>
                        <div>
                            <h6 class="fw-bold mb-0">Professional Networking</h6>
                            <p class="text-muted mb-0">linkedin.com/in/username</p>
                        </div>
                    </div>
                </div>

                <!-- Contact Form -->
                <div class="col-lg-6">
                    <form @submit.prevent="submitForm">
                        <div class="row g-3">
                            <div class="col-md-6">
                                <label class="form-label small fw-bold">Name</label>
                                <input type="text" v-model="name" class="form-control bg-light border-0 py-2" placeholder="Your Name" required>
                            </div>
                            <div class="col-md-6">
                                <label class="form-label small fw-bold">Email Address</label>
                                <input type="email" v-model="email" class="form-control bg-light border-0 py-2" placeholder="name@example.com" required>
                            </div>
                            <div class="col-12">
                                <label class="form-label small fw-bold">Subject</label>
                                <input type="text" v-model="subject" class="form-control bg-light border-0 py-2" placeholder="Project Inquiry" required>
                            </div>
                            <div class="col-12">
                                <label class="form-label small fw-bold">Message</label>
                                <textarea v-model="message" class="form-control bg-light border-0 py-2" rows="4" placeholder="Tell me about your timeline and expectations..." required></textarea>
                            </div>
                            <div class="d-flex justify-content-end mt-2">
                                <div ref="recaptchaContainer"></div>
                            </div>
                            <div class="col-12">
                                <button type="submit" :disabled="isLoading" class="btn btn-emerald px-4 py-2 w-100">{{isLoading ? "Sending..." : "Submit"}}</button>
                            </div>
                       
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
 import { ref } from 'vue';
 import { Notyf } from 'notyf';
 import 'notyf/notyf.min.css';

 const notyf = new Notyf();
 const WEB3FORMS_ACCESS_KEY = "3be53269-561b-4b08-bf38-c7c527e3b375"
//  const subject = "New message from Portfolio Contact Form";
 
 const name = ref("");
 const email = ref("");
 const subject = ref("");
 const message = ref("");
 const isLoading = ref(false);

 const submitForm = async() => {
    
    if(!recaptchaToken.value){
        notyf.error("Please verify that you are not a robot.")
        return
    }

    isLoading.value = true;
    try{
        const response = await fetch("https://api.web3forms.com/submit",{
            method:"POST",
            headers:{
                "Content-Type":"application/json",
                Accept:"application/json",
            },
            body: JSON.stringify({
                access_key: WEB3FORMS_ACCESS_KEY,
                subject: subject.value,
                name: name.value,
                email: email.value,
                message:message.value,
            })
        });
        const result = await response.json();

        if(result.success){
            console.log(result);
            isLoading.value = false;
            notyf.success("Message Sent!")
        }
    }catch(error){
        console.log(error);
        isLoading.value = false;
        notyf.error("Failed to send message.");
    } finally {
        resetRecaptcha();
    }
 }

    const SITE_KEY = '6LeWYoktAAAAAH47w0QKF29-8-ZvV7bL-Qqvlv17';  // Replace with your site key

    const recaptchaContainer = ref(null);
    const recaptchaWidgetId = ref(null);
    const recaptchaToken = ref('');

    // Callback called by reCAPTCHA when successful
    function onRecaptchaSuccess(token) {
    recaptchaToken.value = token;
    }

    // Callback when expired
    function onRecaptchaExpired() {
    recaptchaToken.value = '';
    }

    // Function to render the reCAPTCHA widget
    function renderRecaptcha() {
    if (!window.grecaptcha) {
        console.error('reCAPTCHA not loaded');
        return;
    }

    recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
        sitekey: SITE_KEY,
        size: 'normal', // or 'compact'
        callback: onRecaptchaSuccess,
        'expired-callback': onRecaptchaExpired,
    });
    }

    // Function to reset reCAPTCHA 
    function resetRecaptcha() {
    if (recaptchaWidgetId.value !== null) {
        window.grecaptcha.reset(recaptchaWidgetId.value);
        recaptchaToken.value = '';
    }
    }



    onMounted(() => {
    // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
    // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
    // Callback functions handle success and expiration events. 
    // reCAPTCHA is reset upon form submission to clear the token.
    const interval = setInterval(() => {
        if (window.grecaptcha && window.grecaptcha.render) {
        renderRecaptcha();
        clearInterval(interval);
        }
    }, 100);

    onBeforeUnmount(() => {
        clearInterval(interval);
    });
    });
</script>