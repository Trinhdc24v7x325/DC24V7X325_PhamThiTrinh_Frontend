<template>
  <Form @submit="submitContact" :validation-schema="contactFormSchema">
    <div class="form-group">
      <label for="name">Tên</label>
      <Field
        name="name"
        type="text"
        class="form-control"
        v-model="contactLocal.name"
      />
      <ErrorMessage name="name" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="email">E-mail</label>
      <Field
        name="email"
        type="email"
        class="form-control"
        v-model="contactLocal.email"
      />
      <ErrorMessage name="email" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="address">Địa chỉ</label>
      <Field
        name="address"
        type="text"
        class="form-control"
        v-model="contactLocal.address"
      />
      <ErrorMessage name="address" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="phone">Điện thoại</label>
      <Field
        name="phone"
        type="tel"
        class="form-control"
        v-model="contactLocal.phone"
      />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>
    <div class="form-group">
      <label class="d-block mb-2"><strong>Tình trạng gia đình</strong></label>

      <div class="btn-group" role="group" aria-label="Tình trạng gia đình">
        <Field
          type="radio"
          class="btn-check"
          name="tinhTrangGiaDinh"
          id="tt-doc-than"
          autocomplete="off"
          value="doc_than"
          v-model="contactLocal.tinhTrangGiaDinh"
        />
        <label class="btn btn-outline-primary" for="tt-doc-than"
          >Độc thân</label
        >

        <Field
          type="radio"
          class="btn-check"
          name="tinhTrangGiaDinh"
          id="tt-gia-dinh"
          autocomplete="off"
          value="gia_dinh"
          v-model="contactLocal.tinhTrangGiaDinh"
        />
        <label class="btn btn-outline-primary" for="tt-gia-dinh"
          >Có Gia đình</label
        >
      </div>

      <ErrorMessage
        name="tinhTrangGiaDinh"
        class="error-feedback d-block mt-1"
      />
    </div>
    <div class="form-group">
      <label class="d-block mb-2"
        ><strong>Sở thích</strong>
        <span class="text-muted">(chọn nhiều)</span></label
      >
      <div class="form-check">
        <Field
          class="form-check-input"
          type="checkbox"
          id="st-doc-sach"
          value="doc_sach"
          v-model="contactLocal.soThich"
          name="soThich"
        />
        <label for="st-doc-sach" class="form-check-label">Đi cà phê</label>
      </div>

      <div class="form-check">
        <Field
          class="form-check-input"
          type="checkbox"
          id="st-xem-phim"
          value="xem_phim"
          v-model="contactLocal.soThich"
          name="soThich"
        />
        <label for="st-xem-phim" class="form-check-label">Xem phim</label>
      </div>

      <div class="form-check">
        <Field
          class="form-check-input"
          type="checkbox"
          id="st-choi-dan"
          value="choi_dan"
          v-model="contactLocal.soThich"
          name="soThich"
        />
        <label for="st-choi-dan" class="form-check-label">Chơi đàn</label>
      </div>

      <ErrorMessage name="soThich" class="error-feedback d-block mt-1" />
    </div>

    <div class="form-group form-check">
      <input
        name="favorite"
        type="checkbox"
        class="form-check-input"
        v-model="contactLocal.favorite"
      />
      <label for="favorite" class="form-check-label">
        <strong>Liên hệ yêu thích</strong>
      </label>
    </div>
    <div class="form-group">
      <button class="btn btn-primary">Lưu</button>
      <button
        v-if="contactLocal._id"
        type="button"
        class="ml-2 btn btn-danger"
        @click="deleteContact"
      >
        Xóa
      </button>
      <button type="button" class="ml-2 btn btn-danger" @click="Cancel">
        Thoát
      </button>
    </div>
  </Form>
</template>

<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  emits: ["submit:contact", "delete:contact"],
  props: {
    contact: { type: Object, required: true },
  },
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup
        .string()
        .required("Tên phải có giá trị.")
        .min(2, "Tên phải ít nhất 2 ký tự.")
        .max(50, "Tên có nhiều nhất 50 ký tự."),
      email: yup
        .string()
        .email("E-mail không đúng.")
        .max(50, "E-mail tối đa 50 ký tự."),
      address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự."),
      phone: yup
        .string()
        .matches(
          /((09|03|07|08|05)+([0-9]{8})\b)/g,
          "Số điện thoại không hợp lệ."
        ),
      tinhTrangGiaDinh: yup
        .string()
        .oneOf(["doc_than", "gia_dinh"], "Chọn tình trạng hợp lệ.")
        .required("Vui lòng chọn tình trạng gia đình."),
      soThich: yup
        .array()
        .of(yup.string().oneOf(["doc_sach", "xem_phim", "choi_dan"]))
        .min(1, "Chọn ít nhất 1 sở thích."),
    });
    return {
      contactLocal: this.contact,
      contactFormSchema,
    };
  },
  methods: {
    submitContact() {
      this.$emit("submit:contact", this.contactLocal);
    },
    deleteContact() {
      this.$emit("delete:contact", this.contactLocal.id);
    },
    Cancel() {
      const reply = window.confirm(
        "You have unsaved changes! Do you want to leave?"
      );
      if (!reply) {
        return false;
      } else {
        this.$router.push({ name: "contactbook" });
      }
    },
  },
};
</script>

<style scoped>
@import "@/assets/form.css";
</style>
