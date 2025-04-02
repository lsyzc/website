<script setup lang="ts">
// import raw from '../../../photos/data'
const raw = [
  { url: 'https://pic1.imgdb.cn/item/615c528f2ab3f51d91a62061.jpg', text: 'Image 1 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528f2ab3f51d91a62026.jpg', text: 'Image 2 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528f2ab3f51d91a61ff9.jpg', text: 'Image 3 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528f2ab3f51d91a61fd2.jpg', text: 'Image 4 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528f2ab3f51d91a61fad.jpg', text: 'Image 5 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528f2ab3f51d91a61f62.jpg', text: 'Image 7 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61f3f.jpg', text: 'Image 8 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61f27.jpg', text: 'Image 9 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61ef4.jpg', text: 'Image 10 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61ede.jpg', text: 'Image 11 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61ec3.jpg', text: 'Image 12 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61ea1.jpg', text: 'Image 13 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61e8f.jpg', text: 'Image 14 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61e7f.jpg', text: 'Image 15 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61e6d.jpg', text: 'Image 16 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61e58.jpg', text: 'Image 17 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61e4b.jpg', text: 'Image 18 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61e1a.jpg', text: 'Image 19 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61e0e.jpg', text: 'Image 20 description' },
  { url: 'https://pic1.imgdb.cn/item/615c528e2ab3f51d91a61e06.jpg', text: 'Image 21 description' },
];

const props = defineProps<{
  limit?: number
}>()

const photos = computed(() => {
  if (props.limit)
    return raw.slice(0, props.limit)
  return raw
})
</script>

<template>
  <div class="photos grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4" max-w-500 mx-auto>
    <div v-for="photo, idx in photos" :key="idx">
      <img
        :src="photo.url"
        :alt="photo.text"
        :data-photo-index="idx"
        loading="lazy"
        w-full aspect-square object-cover
      >
    </div>
  </div>
</template>
