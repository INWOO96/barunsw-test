<script>
import { ref, reactive, onMounted, computed } from 'vue'
import useAxios from '/@app_modules/axios.js'

export default {
	setup() {
		const { axiosGet } = useAxios()
		const posts = reactive([])

		const rows = ref(5)
		const total_rows = ref(0)
		const page = ref(1) 

		const total_pages = computed(() => {
			return Math.ceil(total_rows.value / rows.value)
		})
		const sliced_posts = computed(() => {
			return posts.slice((page.value - 1 ) * rows.value, page.value * rows.value)
		})

		onMounted(() => {
			axiosGet('/api/blog', (res) => {
				console.log(res);
				Object.assign(posts, res)
				total_rows.value = posts.length
			})
		})

		return {
			sliced_posts,
			page,
			total_pages
		}
	}	
}
</script>

<template>
	<main class="container">
		<div class="row g-5">
			<div class="col-md-8">
			<h3 class="pb-4 mb-4 fst-italic border-bottom">
				K-Post
			</h3>

			<article class="blog-post" v-for="article in sliced_posts" :key="article.id">
				<h2 class="blog-post-title">{{ article.title }}</h2>
				<p class="blog-post-meta">{{ article.date }}</p>
				<div v-html="article.post"/>
			</article>

			<nav class="blog-pagination" aria-label="Pagination">
				<a class="btn" href="#" :class="[page == 1 ? 'btn-outline-secondary disabled' : 'btn-outline-primary']"
				   @click="page--">Prev</a>
				{{ page }}
				<a class="btn" href="#" :class="[page == total_pages ? 'btn-outline-secondary disabled' : 'btn-outline-primary']"
				   @click="page++">Next</a>
			</nav>

			</div>

			<div class="col-md-4">
			<div class="position-sticky" style="top: 2rem;">
				<div class="p-4 mb-3 bg-light rounded">
				<h4 class="fst-italic">About</h4>
				<p class="mb-0">Customize this section to tell your visitors a little bit about your publication, writers, content, or something else entirely. Totally up to you.</p>
				</div>

				<div class="p-4">
				<h4 class="fst-italic">Archives</h4>
				<ol class="list-unstyled mb-0">
					<li><a href="#">March 2021</a></li>
					<li><a href="#">February 2021</a></li>
					<li><a href="#">January 2021</a></li>
					<li><a href="#">December 2020</a></li>
					<li><a href="#">November 2020</a></li>
					<li><a href="#">October 2020</a></li>
					<li><a href="#">September 2020</a></li>
					<li><a href="#">August 2020</a></li>
					<li><a href="#">July 2020</a></li>
					<li><a href="#">June 2020</a></li>
					<li><a href="#">May 2020</a></li>
					<li><a href="#">April 2020</a></li>
				</ol>
				</div>

				<div class="p-4">
				<h4 class="fst-italic">Elsewhere</h4>
				<ol class="list-unstyled">
					<li><a href="#">GitHub</a></li>
					<li><a href="#">Twitter</a></li>
					<li><a href="#">Facebook</a></li>
				</ol>
				</div>
			</div>
			</div>
		</div>

	</main>
</template>

<style scoped>
.blog-pagination { 
	margin-bottom: 4rem;
}

.blog-pagination > .btn {
	border-radius: 2rem;
}

.blog-post {
	margin-bottom: 4rem;
}
.blog-post-title {
	margin-bottom: 0.25rem;
	font-size: 2.5rem;
}
.blog-post-meta { 
	margin-bottom: 1.25rem;
	color: #727272;
}
</style>