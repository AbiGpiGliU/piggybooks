<template>
	<div class="container">
		<div class="header">
			<div class="header-container">
				<h1>Piggy Books</h1>
				<div class="search-container">
					<div class="search-island">
						<input type="text" v-model="search" />
						<button @click="searchBooks">
							<svg xmlns="http://www.w3.org/2000/svg" height="24" width="24">
								<path
									d="m19.6 21-6.3-6.3q-.75.6-1.725.95Q10.6 16 9.5 16q-2.725 0-4.612-1.887Q3 12.225 3 9.5q0-2.725 1.888-4.613Q6.775 3 9.5 3t4.613 1.887Q16 6.775 16 9.5q0 1.1-.35 2.075-.35.975-.95 1.725l6.3 6.3ZM9.5 14q1.875 0 3.188-1.312Q14 11.375 14 9.5q0-1.875-1.312-3.188Q11.375 5 9.5 5 7.625 5 6.312 6.312 5 7.625 5 9.5q0 1.875 1.312 3.188Q7.625 14 9.5 14Z"
								/>
							</svg>
						</button>
					</div>
				</div>
			</div>
		</div>
		<div class="main">
			<Book v-for="book in books" :key="book.id" :book="book"></Book>
		</div>
	</div>
</template>

<script>
import Book from '../components/Book.vue'
export default {
	components: { Book },
	data() {
		return {
			search: '',
			totalItems: '',
			status: false,
			books: [],
		}
	},
	methods: {
		async searchBooks() {
			if (!this.search) return
			const { data: res, status: status } = await this.$http.get('https://www.googleapis.com/books/v1/volumes', {
				params: {
					q: this.search,
					key: 'AIzaSyAc2XJGI7QdUWx8mwu5cP0qsfAtJ3L75cI',
					maxResults: '8',
				},
			})
			if (status == 200) {
				this.status = true
				this.totalItems = res.totalItems
				this.books = res.items
			}
		},
	},
}
</script>

<style scoped lang="less">
.container {
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
	.header {
		min-width: 1200px;
		width: 100%;
		height: 200px;
		background-color: #fae4e6;
		display: flex;
		justify-content: center;
		align-items: center;
		.header-container {
			width: 1200px;
			height: 100%;
			h1 {
				font-size: 4rem;
				height: 100px;
				text-align: center;
				padding-top: 20px;
			}
			.search-container {
				display: flex;
				justify-content: center;
				width: 100%;
				padding-top: 40px;
				.search-island {
					background-color: #fff;
					width: 500px;
					height: 44px;
					display: flex;
					justify-content: center;
					margin-bottom: 10px;
					border-radius: 20px;
					input {
						display: inline-block;
						padding: 0;
						height: 100%;
						width: 94%;
						background-color: rgba(0, 0, 0, 0);
						border: none;
						text-indent: 15px;
						font: 1em sans-serif;
						&:focus {
							outline: none;
						}
					}
					button {
						display: inline-block;
						width: 33px;
						height: 100%;
						background-color: rgba(0, 0, 0, 0);
						border: none;
						&:hover {
							cursor: pointer;
						}
					}
					&:hover {
						box-shadow: 0 0 5px black;
					}
				}
			}
		}
	}
	.main {
		width: 1200px;
		height: 100%;
	}
}
</style>
