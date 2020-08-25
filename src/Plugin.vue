<template>
  <div>
    <div class="fs-spacing-editor">
      <div class="fs-spacing-editor__bounds" style="background: #fff;">
        <div class="fs-spacing-editor__label">Border</div>
        <div class="fs-spacing-editor__center">
          <select name="bt" v-model="model.bt">
            <option
              v-for="[key, value] in Object.entries(options)"
              :key="model._uid + '_bt_' + value"
              :value="value"
            >{{ key }}</option>
          </select>
        </div>
        <div class="fs-spacing-editor__flex">
          <div>
            <select name="bl" v-model="model.bl">
              <option
                v-for="[key, value] in Object.entries(options)"
                :key="model._uid + '_bl_' + value"
                :value="value"
              >{{ key }}</option>
            </select>
          </div>
          <div class="fs-spacing-editor__center" style="padding: 1em;">Content</div>
          <div>
            <select name="br" v-model="model.br">
              <option
                v-for="[key, value] in Object.entries(options)"
                :key="model._uid + '_br_' + value"
                :value="value"
              >{{ key }}</option>
            </select>
          </div>
        </div>
        <div class="fs-spacing-editor__center">
          <select name="bb" v-model="model.bb">
            <option
              v-for="[key, value] in Object.entries(options)"
              :key="model._uid + '_bb_' + value"
              :value="value"
            >{{ key }}</option>
          </select>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mixins: [window.Storyblok.plugin],
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: "border-editor",
        title: "Border Editor",
        description:
          "Allows the user to set top, right, bottom, and left values for borders.",
      };
    },
    async pluginCreated() {
      if (
        this.schema.source === "internal" &&
        this.schema.datasource_slug &&
        this.schema.datasource_slug.length
      ) {
        const data = await fetchOptionsFromDataSource(this);
        const opts = {};
        this.options = data.map((x) => (opts[x.value] = x.name));
      }
    },
  },
  watch: {
    model: {
      handler: function (value) {
        this.$emit("changed-model", value);
      },
      deep: true,
    },
  },
};

async function fetchOptionsFromDataSource(context) {
  const endpoint = `cdn/datasource_entries?token=${context.token}&datasource=${context.schema.datasource_slug}&per_page=1000`;
  const response = await context.api.get(endpoint);
  return response.data.datasource_entries;
}
</script>
<style>
.fs-spacing-editor {
  /* border: 1px solid #f90; */
  background: #f0f0f0;
  width: 100%;
  overflow: hidden;
}

.fs-spacing-editor select {
  width: 50px;
  overflow: hidden;
}

.fs-spacing-editor__flex {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.fs-spacing-editor__center {
  text-align: center;
  flex-grow: 1;
}

.fs-spacing-editor__bounds {
  position: relative;
  border: 1px dashed #000;
}

.fs-spacing-editor__label,
.fs-spacing-editor__label {
  position: absolute;
  top: 0;
  left: 0;
  padding: 2px 4px;
  font-size: 0.875rem;
}
</style>