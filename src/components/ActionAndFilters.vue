<script setup lang="ts"></script>
<template>
    <div class="actions">
        <button>按價格排序</button>
        <button>按名稱排序</button>
        <select>
            <option value="">按庫存過濾</option>
            <option value="true">有貨</option>
            <option value="false">無貨</option>
        </select>
    </div>
</template>
<style scoped>
.actions {
    display: flex;
    gap: 24px;
}
</style>
