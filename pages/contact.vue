<template>
  <div class="container">
    <h1>{{title}}</h1>
    <h2>{{description}}</h2>

    <!-- Email API form submission -->
    <form
      @submit="handleRequest"
      action="https://formspree.io/markperez1904@gmail.com"
      method="POST"
    >
      <!-- Name nad Email -->
      <section class="field control columns is-centered">
        <input
          type="text"
          name="name"
          class="input column is-3"
          placeholder="Name"
          aria-label="full name"
          required
        />
        <br />
        <input
          type="email"
          name="email"
          class="input column is-3"
          placeholder="Email"
          aria-label="email address"
          required
        />
      </section>
      <br />

      <!-- Message -->
      <section class="field control columns is-centered">
        <div class="column is-6">
          <textarea
            class="textarea"
            name="message"
            placeholder="Message..."
            aria-label="message"
            required
          ></textarea>
        </div>
      </section>
      <br />

      <!-- Submit -->
      <section class="field control columns is-centered is-mobile">
        <input type="submit" value="Submit" class="button is-black" />
      </section>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: 'Contact',
      description:
        'Feel free to send me an email. I will get back to them in no time!'
    }
  },

  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.description
        },
        {
          hid: 'og:type', // open graph type
          name: 'og:type',
          content: 'website'
        },
        {
          hid: 'apple-mobile-web-app-title', // open graph mobile web app title
          name: 'apple-mobile-web-app-title',
          content: this.title
        },
        {
          hid: 'og:title', // open graph title
          name: 'og:title',
          content: this.title
        },
        {
          hid: 'og:description', // open graph description
          name: 'og:description',
          content: this.description
        },
        {
          hid: 'og:url', // open graph url
          name: 'og:url',
          content: 'https://markperez.dev' + this.$route.fullPath
        },
        {
          hid: 'og:image', // open graph image
          name: 'og:image',
          content: 'https://markperez.dev/_nuxt/img/ec3ab3b.png'
        }
      ]
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 30px;
  padding-top: 0.5rem;
}

h2,
p {
  margin: 2rem 0;
}

/* add transitions to input boxes */
input,
textarea {
  transition: all 0.2s;
  background-color: #edf2f7;
}

/* adopt color scheme to input boxes */
input:focus,
textarea:focus {
  transition: all 0.2s;
  border-color: #00c58e;
}

textarea:active {
  border-color: #00c58e;
}

.container {
  padding: 0 2rem;
}

.hidden {
  display: none;
}
</style>
