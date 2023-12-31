<template>
  <nav class="k-browser">
    <div class="k-browser-items">
      <label v-for="item in items" :key="item.value" class="k-browser-item">
        <input
          :checked="selected === item.value"
          :name="name"
          :type="type"
          @change="$emit('select', item)"
        />
        <k-item-image
          v-if="item.image"
          :image="{ ...item.image, cover: true, back: 'black' }"
          class="k-browser-item-image"
        />
        <span class="k-browser-item-info">
          {{ item.label }}
        </span>

        <k-button
          v-if="editable"
          class="k-files-view-status"
          variant="none"
          style="--icon-size: 12px"
          icon="edit"
          :link="item.link"
        />
        <k-dropdown v-if="editable" class="k-files-view-options">
          <k-button
            :responsive="true"
            icon="cog"
            @click="$refs.settings[item.index].toggle()"
          />
          <k-dropdown-content ref="settings" :options="$dropdown(item.link)" />
        </k-dropdown>
      </label>
    </div>
  </nav>
</template>

<script>
export default {
  dialog: "k-files-dialog",
  props: {
    items: {
      type: Array,
      default() {
        return [];
      },
    },
    name: {
      default: "items",
      type: String,
    },
    selected: {
      type: String,
      default: "",
    },
    editable: {
      type: Boolean,
    },
    type: {
      default: "radio",
      type: String,
    },
  },
};
</script>

<style>
.k-browser {
  container-type: inline-size;
  font-size: var(--text-sm);
}

.k-browser-items {
  --browser-item-gap: 1px;
  --browser-item-size: 1fr;
  --browser-item-height: var(--height-sm);
  --browser-item-padding: 0.25rem;
  --browser-item-rounded: var(--rounded);
  display: grid;
  column-gap: var(--browser-item-gap);
  row-gap: var(--browser-item-gap);
  grid-template-columns: repeat(
    auto-fill,
    minmax(var(--browser-item-size), 1fr)
  );
}

.k-browser-item {
  display: flex;
  overflow: hidden;
  gap: 0.5rem;
  align-items: center;
  flex-shrink: 0;
  height: var(--browser-item-height);
  padding-inline: calc(var(--browser-item-padding) + 1px);
  border-radius: var(--browser-item-rounded);
  white-space: nowrap;
  cursor: pointer;
}

.k-browser-item-image {
  height: calc(var(--browser-item-height) - var(--browser-item-padding) * 2);
  aspect-ratio: 1/1;
  border-radius: var(--rounded-sm);
  box-shadow: var(--shadow);
  flex-shrink: 0;
}
/** TODO: .k-browser-item-image:has(svg) */
.k-browser-item-image.k-icon-frame {
  box-shadow: none;
  background: var(--color-white);
}
.k-browser-item-info {
  width: 100%;
  overflow: hidden; 
  text-overflow: ellipsis;
  white-space: nowrap;
}

.k-browser-item-image svg {
  transform: scale(0.8);
}

.k-browser-item input {
  position: absolute;
  box-shadow: var(--shadow);
  opacity: 0;
  width: 0;
}
.k-browser-item:has(input:checked) {
  color: var(--color-blue-700);
  background: var(--color-blue-200);
}
</style>
