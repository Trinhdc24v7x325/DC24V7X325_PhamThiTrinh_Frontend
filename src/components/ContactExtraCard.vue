<template>
  <div class="card extra-card">
    <div class="card-body py-2">
      <div class="row">
        <div class="col-12 mb-1">
          <strong>Tình trạng gia đình:</strong>
          {{ tinhTrangLabel(contact?.tinhTrangGiaDinh) }}
        </div>

        <div class="col-12">
          <strong>Sở thích:</strong>
          <template v-if="contact?.soThich && contact.soThich.length">
            <span
              v-for="st in contact.soThich"
              :key="st"
              class="badge badge-info mr-1"
            >
              {{ soThichLabel(st) }}
            </span>
          </template>
          <span v-else>Không có</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ContactExtraCard",
  props: {
    contact: { type: Object, required: true },
  },
  methods: {
    tinhTrangLabel(code) {
      const map = { doc_than: "Độc thân", gia_dinh: "Có Gia đình" };
      return map[code] || "Không rõ";
    },
    soThichLabel(code) {
      const map = {
        doc_sach: "Đi cà phê",
        xem_phim: "Xem phim",
        choi_dan: "Chơi đàn",
      };
      return map[code] || code;
    },
  },
};
</script>

<style scoped>
.extra-card {
  position: relative;
  z-index: 1;
  pointer-events: auto;
}
</style>
