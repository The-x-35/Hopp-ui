<template>
  <Story title="Link">
    <Variant title="Text Link">
      <HoppSmartLink :to="link" :blank="true"> Click here </HoppSmartLink>
    </Variant>
    <Variant title="Button Link">
      <HoppSmartLink :to="link" :blank="true">
        <HoppButton label="Click here" />
      </HoppSmartLink>
    </Variant>
  </Story>
</template>

<script setup lang="ts">
import { HoppSmartLink } from "../components/smart"
import { HoppButton } from "../components/button"
import { ref } from "vue"

const link = ref("/")
</script>
