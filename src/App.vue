<script setup>
import { computed, ref } from 'vue'

const artistName = 'Your Name'
const whatsappNumber = '254700000000' // Replace with your WhatsApp number, country code first, no + sign.
const activeFilter = ref('All works')

const artworks = ref([
  {
    id: 1,
    title: 'Quiet Strength',
    category: 'Portraits',
    medium: 'Graphite on paper',
    size: 'A3 · 2025',
    price: 8500,
    status: 'Available',
    image: 'https://images.unsplash.com/photo-1541961017774-22349e4a1262?auto=format&fit=crop&w=1000&q=85',
    alt: 'Expressive contemporary artwork in a warm gallery setting'
  },
  {
    id: 2,
    title: 'After the Rain',
    category: 'Studies',
    medium: 'Charcoal study',
    size: 'A4 · 2025',
    price: 6200,
    status: 'Available',
    image: 'https://images.unsplash.com/photo-1579783902614-a3fb3927b6a5?auto=format&fit=crop&w=1000&q=85',
    alt: 'Framed fine art study'
  },
  {
    id: 3,
    title: 'The In-Between',
    category: 'Portraits',
    medium: 'Graphite and ink',
    size: 'A3 · 2024',
    price: 9500,
    status: 'Sold',
    image: 'https://images.unsplash.com/photo-1577083552431-6e5fd01aa342?auto=format&fit=crop&w=1000&q=85',
    alt: 'Modern artwork displayed on a gallery wall'
  },
  {
    id: 4,
    title: 'Soft Geometry',
    category: 'Studies',
    medium: 'Mixed media on paper',
    size: 'A4 · 2025',
    price: 4800,
    status: 'Available',
    image: 'https://images.unsplash.com/photo-1578301978693-85fa9c0320b9?auto=format&fit=crop&w=1000&q=85',
    alt: 'Abstract artwork with a minimal composition'
  },
])

const filters = ['All works', 'Portraits', 'Studies']
const filteredArtworks = computed(() => {
  if (activeFilter.value === 'All works') return artworks.value
  return artworks.value.filter((artwork) => artwork.category === activeFilter.value)
})

const formatPrice = (price) =>
  new Intl.NumberFormat('en-KE', { maximumFractionDigits: 0 }).format(price)

function orderArtwork(artwork) {
  if (artwork.status === 'Sold') return
  const message = `Hello! I'm interested in purchasing "${artwork.title}" listed at KSh ${formatPrice(artwork.price)}. Is it still available?`
  window.open(`https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`, '_blank', 'noopener,noreferrer')
}

function contactArtist() {
  const message = 'Hello! I found your art portfolio and would love to ask about your artwork.'
  window.open(`https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`, '_blank', 'noopener,noreferrer')
}
</script>

<template>
  <div class="site-shell">
    <header class="topbar">
      <a class="wordmark" href="#home" aria-label="Atelier Noire home">
        <span class="wordmark-mark">A.</span>
        <span>ATELIER <span class="wordmark-light">NOIRE</span></span>
      </a>

      <nav class="desktop-nav" aria-label="Main navigation">
        <a href="#gallery">The collection</a>
        <a href="#about">The artist</a>
        <a href="#contact">Get in touch</a>
      </nav>

      <a class="nav-cta" href="#gallery">Explore works <span aria-hidden="true">↗</span></a>
    </header>

    <main>
      <section id="home" class="hero section-wrap">
        <div class="hero-copy">
          <p class="eyebrow"><span class="eyebrow-line"></span> ORIGINAL ART · MADE WITH INTENTION</p>
          <h1>Art that makes<br />you <em>feel something.</em></h1>
          <p class="hero-description">
            Thoughtful drawings, honest details, and little pieces of a bigger story.
            Find an original work that feels like it belongs with you.
          </p>
          <div class="hero-actions">
            <a class="button button-primary" href="#gallery">Discover the collection <span aria-hidden="true">↘</span></a>
            <a class="text-link" href="#about">A little about me <span aria-hidden="true">↗</span></a>
          </div>
          <div class="hero-note">
            <span class="note-star">✳</span>
            <p>Every piece has a story.<br /><span>Every wall deserves one.</span></p>
          </div>
        </div>

        <div class="hero-art">
          <div class="hero-art-frame">
            <img
              src="https://images.unsplash.com/photo-1577083552431-6e5fd01aa342?auto=format&fit=crop&w=1200&q=90"
              alt="Featured contemporary artwork in a gallery"
            />
          </div>
          <div class="art-index"><span>01</span> / FEATURED WORK</div>
          <div class="floating-caption">
            <span class="caption-label">A STUDY IN STILLNESS</span>
            <span class="caption-title">The art of slowing down.</span>
          </div>
          <div class="hero-vertical">AN ORIGINAL POINT OF VIEW</div>
        </div>

        <div class="hero-bottomline">
          <span>INDEPENDENT ARTIST · NAIROBI, KENYA</span>
          <a href="#gallery">SCROLL TO EXPLORE <span aria-hidden="true">↓</span></a>
        </div>
      </section>

      <section id="gallery" class="gallery-section section-wrap">
        <div class="section-heading">
          <div>
            <p class="eyebrow"><span class="eyebrow-line"></span> THE COLLECTION</p>
            <h2>Works with <em>a little soul.</em></h2>
          </div>
          <p class="section-intro">Original pieces made by hand, one thoughtful mark at a time. Choose a work to make your own.</p>
        </div>

        <div class="gallery-toolbar">
          <div class="filter-list" aria-label="Filter artwork">
            <button
              v-for="filter in filters"
              :key="filter"
              class="filter-button"
              :class="{ active: activeFilter === filter }"
              type="button"
              @click="activeFilter = filter"
            >
              {{ filter }}
            </button>
          </div>
          <span class="work-count">{{ filteredArtworks.length }} WORKS</span>
        </div>

        <div class="art-grid">
          <article v-for="(artwork, index) in filteredArtworks" :key="artwork.id" class="art-card">
            <div class="art-image-wrap">
              <img :src="artwork.image" :alt="artwork.alt" loading="lazy" />
              <span class="status-pill" :class="artwork.status === 'Sold' ? 'status-sold' : 'status-available'">
                <span class="status-dot"></span>{{ artwork.status }}
              </span>
              <span class="image-number">0{{ index + 1 }}</span>
              <button
                class="image-order"
                type="button"
                :disabled="artwork.status === 'Sold'"
                :aria-label="artwork.status === 'Sold' ? `${artwork.title} is sold` : `Ask about ${artwork.title}`"
                @click="orderArtwork(artwork)"
              >
                {{ artwork.status === 'Sold' ? 'Collected' : 'Enquire ↗' }}
              </button>
            </div>
            <div class="art-card-details">
              <div>
                <h3>{{ artwork.title }}</h3>
                <p>{{ artwork.medium }} <span>·</span> {{ artwork.size }}</p>
              </div>
              <div class="art-price">
                <span class="price-label">PRICE</span>
                <strong>KSh {{ formatPrice(artwork.price) }}</strong>
              </div>
            </div>
          </article>
        </div>

        <p class="gallery-footnote"><span>✳</span> All works are unique. Prices are listed in Kenyan shillings and may exclude delivery.</p>
      </section>

      <section id="about" class="about-section">
        <div class="about-image">
          <img
            src="https://images.unsplash.com/photo-1513364776144-60967b0f800f?auto=format&fit=crop&w=1200&q=85"
            alt="Artist's workspace with art materials"
            loading="lazy"
          />
          <div class="image-stamp">MADE<br />BY HAND <span>✳</span></div>
        </div>
        <div class="about-copy">
          <p class="eyebrow"><span class="eyebrow-line"></span> A NOTE FROM THE ARTIST</p>
          <h2>Made slowly.<br /><em>Felt deeply.</em></h2>
          <p class="about-lead">
            Hi, I'm {{ artistName }} — an artist who believes the smallest details can hold the biggest feelings.
          </p>
          <p class="about-body">
            My work begins with curiosity: a face, a fleeting moment, a line that doesn't need to be perfect.
            Each piece is created by hand, with room for texture, imperfection, and the unexpected.
          </p>
          <p class="about-body">
            This little collection is an invitation to live with art that feels personal. Thank you for being here
            and for supporting independent art.
          </p>
          <a class="text-link about-link" href="#contact">Let's talk about art <span aria-hidden="true">↗</span></a>
          <div class="artist-signature">{{ artistName }}</div>
        </div>
      </section>

      <section id="contact" class="contact-section section-wrap">
        <div class="contact-orbit orbit-one"></div>
        <div class="contact-orbit orbit-two"></div>
        <div class="contact-content">
          <p class="eyebrow"><span class="eyebrow-line"></span> YOUR WALL, YOUR STORY</p>
          <h2>Found the one?<br /><em>Let's make it yours.</em></h2>
          <p>Ask about availability, shipping, commissions, or anything else on your mind. I'd love to hear from you.</p>
          <button class="button button-primary contact-button" type="button" @click="contactArtist">
            Start a conversation <span aria-hidden="true">↗</span>
          </button>
          <span class="contact-small">A REAL PERSON ON THE OTHER SIDE. ALWAYS.</span>
        </div>
        <div class="contact-decoration" aria-hidden="true">
          <span class="deco-cross">✳</span>
          <span class="deco-circle"></span>
          <span class="deco-line"></span>
        </div>
      </section>
    </main>

    <footer class="footer section-wrap">
      <a class="wordmark footer-wordmark" href="#home">
        <span class="wordmark-mark">A.</span>
        <span>ATELIER <span class="wordmark-light">NOIRE</span></span>
      </a>
      <p>Original art, made with intention.</p>
      <div class="footer-right">
        <a href="#home">BACK TO TOP ↑</a>
        <span>© {{ new Date().getFullYear() }} {{ artistName }}</span>
      </div>
    </footer>
  </div>
</template>
