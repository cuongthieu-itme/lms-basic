<script setup>
import { ref } from 'vue';
import { useForm } from '@inertiajs/vue3';
import ActionSection from '@/Components/ActionSection.vue';
import DangerButton from '@/Components/DangerButton.vue';
import DialogModal from '@/Components/DialogModal.vue';
import InputError from '@/Components/InputError.vue';
import SecondaryButton from '@/Components/SecondaryButton.vue';
import TextInput from '@/Components/TextInput.vue';

const confirmingUserDeletion = ref(false);
const passwordInput = ref(null);

const form = useForm({
    password: '',
});

const confirmUserDeletion = () => {
    confirmingUserDeletion.value = true;

    setTimeout(() => passwordInput.value.focus(), 250);
};

const deleteUser = () => {
    form.delete(route('current-user.destroy'), {
        preserveScroll: true,
        onSuccess: () => closeModal(),
        onError: () => passwordInput.value.focus(),
        onFinish: () => form.reset(),
    });
};

const closeModal = () => {
    confirmingUserDeletion.value = false;

    form.reset();
};
</script>

<template>
    <ActionSection>
        <template #title>
            Xóa tài khoản
        </template>

        <template #description>
            Xóa vĩnh viễn tài khoản của bạn.
        </template>

        <template #content>
            <div class="max-w-xl text-sm text-gray-600">
                Khi tài khoản bị xóa, tất cả dữ liệu và tài nguyên liên quan sẽ bị xóa vĩnh viễn. Trước khi xóa, hãy tải xuống mọi dữ liệu bạn muốn lưu lại.
            </div>

            <div class="mt-5">
                <DangerButton @click="confirmUserDeletion">
                    Xóa tài khoản
                </DangerButton>
            </div>

            <!-- Delete Account Confirmation Modal -->
            <DialogModal :show="confirmingUserDeletion" @close="closeModal">
                <template #title>
                    Xóa tài khoản
                </template>

                <template #content>
                    Bạn có chắc chắn muốn xóa tài khoản? Sau khi xóa, tất cả dữ liệu và tài nguyên sẽ bị xóa vĩnh viễn. Vui lòng nhập mật khẩu để xác nhận.

                    <div class="mt-4">
                        <TextInput
                            ref="passwordInput"
                            v-model="form.password"
                            type="password"
                            class="mt-1 block w-3/4"
                            placeholder="Mật khẩu"
                            autocomplete="current-password"
                            @keyup.enter="deleteUser"
                        />

                        <InputError :message="form.errors.password" class="mt-2" />
                    </div>
                </template>

                <template #footer>
                    <SecondaryButton @click="closeModal">
                        Hủy
                    </SecondaryButton>

                    <DangerButton
                        class="ms-3"
                        :class="{ 'opacity-25': form.processing }"
                        :disabled="form.processing"
                        @click="deleteUser"
                    >
                        Xóa tài khoản
                    </DangerButton>
                </template>
            </DialogModal>
        </template>
    </ActionSection>
</template>
