<template>
  <div class="text-center mb-6">
    {{
      new Date().toLocaleDateString('en-us', {
        weekday: 'long',
        month: 'long',
        year: 'numeric',
        day: 'numeric'
      })
    }}
  </div>
</template>
