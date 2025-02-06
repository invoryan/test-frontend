<script setup lang="ts">
import { UserResponse } from "@/models/user/types";
import { ValidationError } from "@/models/validation";
interface Props {
  userData: UserResponse;
  isDialogVisible: boolean;
}

interface Emit {
  (e: "submit", value: UserResponse): void;
  (e: "update:isDialogVisible", val: boolean): void;
}

const props = defineProps<Props>();

const emit = defineEmits<Emit>();
const error = ref<ValidationError>({});
const userData = ref<UserResponse>(structuredClone(toRaw(props.userData)));
const isPasswordVisible = ref(false);

watch(props, () => {
  userData.value = structuredClone(toRaw(props.userData));
});

const onFormSubmit = () => {
  emit("update:isDialogVisible", false);
  emit("submit", userData.value);
};

const onFormReset = () => {
  userData.value = structuredClone(toRaw(props.userData));

  emit("update:isDialogVisible", false);
};

const dialogModelValueUpdate = (val: boolean) => {
  emit("update:isDialogVisible", val);
};
</script>

<template>
  <VDialog
    :width="$vuetify.display.smAndDown ? 'auto' : 900"
    :model-value="props.isDialogVisible"
    @update:model-value="dialogModelValueUpdate"
  >
    <!-- Dialog close btn -->
    <DialogCloseBtn @click="dialogModelValueUpdate(false)" />

    <VCard class="pa-sm-10 pa-2">
      <VCardText>
        <!-- 👉 Title -->
        <h4 class="text-h4 text-center mb-2">Edit User Information</h4>
        <p class="text-body-1 text-center mb-6">
          Updating user details will receive a privacy audit.
        </p>

        <!-- 👉 Form -->
        <VForm class="mt-6" @submit.prevent="onFormSubmit">
          <VRow>
            <!-- 👉 First Name -->
            <VCol cols="12" md="6">
              <AppTextField
                v-model="userData.name"
                label="First Name"
                placeholder="John"
                :rules="[requiredValidator]"
                :error="!!error.name"
                :error-messages="error.name ? error.name[0] : ''"
              />
            </VCol>

            <!-- 👉 Billing Email -->
            <VCol cols="12" md="6">
              <AppTextField
                v-model="userData.email"
                label="Email"
                placeholder="johndoe@email.com"
              />
            </VCol>

            <VCol cols="12">
              <AppTextField
                v-model="userData.password"
                :type="isPasswordVisible ? 'text' : 'password'"
                :append-inner-icon="
                  isPasswordVisible ? 'tabler-eye-off' : 'tabler-eye'
                "
                @click:append-inner="isPasswordVisible = !isPasswordVisible"
                label="Password"
                :error="!!error.password"
                :error-messages="error.password ? error.password[0] : ''"
              />
            </VCol>

            <!-- 👉 Submit and Cancel -->
            <VCol cols="12" class="d-flex flex-wrap justify-center gap-4">
              <VBtn type="submit"> Submit </VBtn>

              <VBtn color="secondary" variant="tonal" @click="onFormReset">
                Cancel
              </VBtn>
            </VCol>
          </VRow>
        </VForm>
      </VCardText>
    </VCard>
  </VDialog>
</template>
