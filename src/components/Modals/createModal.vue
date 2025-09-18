<template>
    <div class="modal fade" id="newModal" tabindex="-1" aria-labelledby="newModal" aria-hidden="true">
        <div class="modal-dialog">
            <VForm class="modal-content" ref="kt_modal_create_form" @submit="addNewItem" :validation-schema="NOTE_UPDATE_CREATE">
                <div class="modal-header">
                    <h5 class="modal-title">New Book <i class="bi bi-clipboard-check-fill"></i></h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" ref="closeModal"></button>
                </div>
                <div class="modal-body text-start">
                    <div class="mb-3">
                        <label for="title" class="form-label">Title <span class="text-danger">*</span></label>
                        <Field class="form-control form-control-solid" type="text" name="title" placeholder="Book title" ref="titleInput" />
                        <div class="fv-plugins-message-container">
                            <div class="fv-help-block text-danger">
                                <ErrorMessage name="title" />
                            </div>
                        </div>
                    </div>
                    <div class="mb-3">
                        <label for="description" class="form-label">Description <span class="text-danger">*</span></label>
                        <Field class="form-control form-control-solid" type="text" name="description" v-model="description"/>
                        <div class="fv-plugins-message-container">
                            <div class="fv-help-block text-danger">
                                <ErrorMessage name="description" />
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" @click="closeModal" :disabled="awaitProccess">Close</button>
                    <button type="button" class="btn btn-loading" disabled v-if="awaitProccess">
                        <div class="spinner-border" style="width: 1rem; height: 1rem; border-width: 2px;" role="status">
                            <span class="visually-hidden">Loading...</span>
                        </div>
                    </button>
                    <button type="submit" class="btn btn-primary" v-else>Save changes</button>
                </div>
            </VForm>
        </div>
    </div>
</template>

<script lang="js">
import { ErrorMessage, Field, Form as VForm } from "vee-validate";
import { useDashboardStore } from "@/stores/dashboard.js"
import ToastHelper from "@/config/ToastHelper.js";
import { NOTE_UPDATE_CREATE } from "@/helpers/schemas.js"

export default {
    components: {
        ErrorMessage,
        Field,
        VForm
    },
    data() {
        return {
            description: '',
            awaitProccess: false,
        }
    },
    setup() {
        const store = useDashboardStore()

        return {
            store,
            NOTE_UPDATE_CREATE,
        }
    },
    methods: {
        async addNewItem(values) {
            this.awaitProccess = true;

            await this.store.addNewItem(values);

            this.awaitProccess = false;
            if (this.store.error.length > 0) {
                ToastHelper.error(this.store.error[0])
                return;
            }

            ToastHelper.success('Added with sucessful');
            this.closeModal();
        },
        setEditorRef(emit) {
            this.qlEditorRef = emit
        },
        clearForm() {
            this.description = '';
            this.$refs['titleInput'].value = '';

            // clear quill editor
            // document.querySelector('.ql-editor').innerHTML = '';
        },
        async closeModal() {
            this.clearForm();
            this.$refs['closeModal'].click();
            await this.$parent.getAll();

            this.$refs['kt_modal_create_form'].resetForm();
        }
    },
    watch: {
        'description': {
            handler(newVal) {
                if (newVal === '<p><br></p>') {
                    this.description = ''
                }
            }
        }
    }
}
</script>

<style scoped>
.modal-content {
    background-color: white;
    border: 1px solid #ddd;
    border-radius: 12px;
    color: #34495e;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

.modal-header {
    border-bottom: 1px solid #f1f1f1;
    padding: 20px 25px;
}

.modal-title {
    font-size: 1.25rem;
    font-weight: 600;
    color: #2c3e50;
    margin: 0;
}

.btn-close {
    opacity: 0.7;
    transition: opacity 0.2s;
}

.btn-close:hover {
    opacity: 1;
}

.modal-body {
    padding: 25px;
}

.form-label {
    color: #34495e !important;
    font-weight: 600 !important;
    margin-bottom: 8px !important;
    display: block !important;
}

.form-control {
    height: 35px !important;
    border-radius: 8px !important;
    border: 1px solid #ddd !important;
    padding: 0 15px !important;
    transition: all 0.3s ease !important;
    color: #34495e;
}

.form-control:focus {
    border-color: #3498db !important;
    box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.1) !important;
    outline: none !important;
}

.modal-footer {
    border-top: 1px solid #f1f1f1;
    padding: 15px 25px;
    display: flex;
    gap: 10px;
}

.btn {
    border-radius: 8px !important;
    font-weight: 600 !important;
    transition: all 0.3s ease !important;
    padding: 8px 16px !important;
}

.btn-secondary {
    background: #ecf0f1 !important;
    border: none !important;
    color: #34495e !important;
}

.btn-secondary:hover {
    background: #dcdde1 !important;
    transform: translateY(-2px) !important;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1) !important;
}

.btn-primary {
    background: #3498db !important;
    border: none !important;
    color: white !important;
}

.btn-primary:hover {
    background: #2980b9 !important;
    transform: translateY(-2px) !important;
    box-shadow: 0 4px 8px rgba(52, 152, 219, 0.2) !important;
}

.btn-loading {
    background: #ecf0f1 !important;
    color: #95a5a6 !important;
    border: none !important;
    display: flex;
    align-items: center;
    justify-content: center;
}

.text-danger {
    color: #e74c3c !important;
    font-size: 13px !important;
}

.fv-help-block {
    margin-top: 5px !important;
}
</style>