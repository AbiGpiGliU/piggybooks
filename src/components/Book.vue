<template>
	<div class="book-container">
		<div class="book-island">
			<div class="img-container">
				<img class="lazy" height="100%" :data-src="book.volumeInfo.imageLinks.smallThumbnail" alt="" />
			</div>
			<div class="bookinfo-container">
				<h1>{{ title }}</h1>
				<p>作者:{{ author }}</p>
				<p>出版商:{{ publisher }}</p>
				<p>出版日期:{{ publishedDate }}</p>
				<p class="description">描述:{{ description }}</p>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: ['book'],
    computed:{
		title(){
			return this.book.volumeInfo.title?this.book.volumeInfo.title:'unknown'
		},
		author(){
			return this.book.volumeInfo.author?this.book.volumeInfo.author:'unknown'
		},
		publisher(){
			return this.book.volumeInfo.publisher?this.book.volumeInfo.publisher:'unknown'
		},
		publishedDate(){
			return this.book.volumeInfo.publishedDate?this.book.volumeInfo.publishedDate:'unknown'
		},
		description(){
			let str = this.book.volumeInfo.description
			return str.length>300?this.book.volumeInfo.description = str.substring(0,300)+'...':str
		}
    },
	mounted(){
		this.lazyLoad()
	},
	methods:{
		lazyLoad(){
			let lazyLoadImages = document.querySelectorAll('.lazy')
			let imageObserver = new IntersectionObserver((entries)=>{
				entries.forEach(function(entry){
					if(entry.isIntersecting){
						let image = entry.target
						image.setAttribute('src',image.getAttribute('data-src'))
						image.classList.remove('lazy')
						imageObserver.unobserve(image)
					}
				})
			})
			lazyLoadImages.forEach(function(image){
				imageObserver.observe(image)
			})
		}
	}
}
</script>

<style scoped lang="less">
.book-container {
	height: 240px;
	width: 1200px;
	padding: 20px;
	p{
		margin:10px 0;
	}
	.book-island {
		display: flex;
		justify-content: start;
		align-items: center;
		height: 100%;
		border: 2px solid #fae4e6;
		border-radius: 20px;
		&:hover {
			box-shadow: 0 0 10px pink;
		}
		.img-container {
			width: 128px;
			min-width: 128px;
			height: 159px;
			margin-left: 10px;
            overflow: hidden;
            img{
                width: 100%;
            }
		}
		.bookinfo-container {
			height: 100%;
			max-width: 1000px;
			overflow: hidden;
			padding: 5px;
			.description{
				line-height: 24px;
			}
		}
	}
}
</style>
