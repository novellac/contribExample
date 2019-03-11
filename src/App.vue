<template>
  <div id="app" class="content">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title column is-half-mobile">Table Sort + Filter Example</h1>
      </div>
      <div class="column is-12 columns is-multiline">
        <div class="field column">
          <label class="label">Search by Title</label>
          <div class="control">
            <input
              class="input"
              type="text"
              placeholder="Great Cats of Web Development"
              v-model="searchTitle"
            />
          </div>
          <p class="help">This is case sensitive.</p>
        </div>
        <div class="field column">
          <label class="label">Search by Author</label>
          <div class="control">
            <input
              class="input"
              type="text"
              placeholder="@exampleAuthor / Maria Mao"
              v-model="searchAuthor"
            />
          </div>
          <p class="help">This is case sensitive.</p>
        </div>
        <div class="field column">
          <label class="label">Publishing Status</label>
          <div class="control">
            <div class="select">
              <select v-model="selectedContentStatus">
                <option selected value> - all - </option>
                <option
                  v-for="item in contentStatus"
                  v-bind:key="item.key"
                  v-bind:value="item.key"
                  >{{ item.label }}</option
                >
              </select>
            </div>
          </div>
        </div>
        <div class="field column">
          <label class="label">Content Type</label>
          <div class="control">
            <div class="select">
              <select v-model="selectedContentType">
                <option selected value> - all - </option>
                <option
                  v-for="(item, index) in contentType"
                  v-bind:key="index"
                  >{{ item }}</option
                >
              </select>
            </div>
          </div>
        </div>
        <div class="column is-12">
          <div class="field-label is-normal has-text-left">
            <label class="label">Tags</label>
          </div>
          <div class="field is-grouped is-grouped-multiline">
            <div
              v-for="(tag, index) in contentTags"
              v-bind:key="index"
              class="control"
            >
              <div class="tags">
                <a
                  class="tag is-link is-medium"
                  :class="tagClasses(tag)"
                  @click="addRemoveTag(tag);"
                  >{{ tag }}</a
                >
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <table
      class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth"
    >
      <thead>
        <th>Status</th>
        <th>Contribution</th>
        <th>Tags</th>
        <th>
          Publication Info <br />
          (Created / Updated)
        </th>
      </thead>
      <tbody>
        <tr v-for="(tablerow, index) in filteredContent" v-bind:key="index">
          <td>
            <p>
              {{ statusNames(tablerow.content_status)
              }}<span v-if="tablerow.content_status == 1">
                ({{ tablerow.publication_date }})
              </span>
            </p>
          </td>
          <td>
            <a class="is-size-4" :href="tablerow.url">{{ tablerow.title }}</a
            ><a class="button is-small is-rounded">{{
              tablerow.content_type
            }}</a>
            <p>
              <span
                v-for="(author, index) in tablerow.authors"
                v-bind:key="index"
                class="is-size-6 has-text-grey"
                ><a
                  :href="author.website"
                  class="is-size-6 has-text-grey has-text-uppercaase"
                  >{{ author.name }}</a
                >
                (<a
                  :href="'http://twitter.com/' + author.twitter"
                  class="is-size-6 has-text-grey"
                  >{{ author.twitter }}</a
                >)<span v-if="index !== tablerow.authors.length - 1"
                  >,&nbsp;</span
                >
              </span>
            </p>
          </td>
          <td>
            <div class="tags">
              <span v-for="(tag, index) in tablerow.tags" v-bind:key="index">
                <a @click="addRemoveTag(tag);" :class="tagClasses(tag)">{{
                  tag
                }}</a>
                <span v-if="index !== tablerow.tags.length - 1"
                  >&nbsp;|&nbsp;</span
                >
              </span>
            </div>
          </td>
          <td>
            <span>Created: {{ tablerow.created_date }}</span> <br />Updated:
            {{ tablerow.updated_date }}
          </td>
        </tr>
      </tbody>
    </table>
    <h2>My Goals</h2>
    <ul>
      <li>x Sort by title</li>
      <li>
        ? Add Publishing status filter (this is not working when content_status
        is 0)
      </li>
      <li>x Add content type filter</li>
      <li>Add filter by author</li>
      <li>(Stretch goal) Move Tags filter into a component</li>
    </ul>
    <h2>My Tasks & Questions</h2>
    <ul>
      <li>
        Make tags list gather all possible tags from json (do not hard code in
        data)
      </li>
      <li>Sort by created by clicking on "created" in the table header</li>
      <li>Sort by created by clicking on "updated" in the table header</li>
      <li>
        Search by author's name or author's twitter handle using search input
      </li>
    </ul>
    <h2>Tasks Done</h2>
    <ul>
      <li>Filter by publication status by using dropdown</li>
      <li>
        Assign names to content status numbers (maybe use a computed property?)
        Create component with status label and do the logic in computed property
        there.
      </li>
      <li>
        Click on multiple tags to show items which have all selected terms.
      </li>
      <li>Search by title using search input</li>
      <li>
        Click on tag's "x" to remove from filters. (Eventually removed the x's
        because did click/unclick)
      </li>
      <li>Sorted by title</li>
      <li>Combined addTag and removeTag into one addRemoveTag filter</li>
    </ul>
  </div>
</template>

<script>
import contribContent from "./data/contribContent.json";

export default {
  contribContent: contribContent,
  name: "App",
  data() {
    return {
      contentStatus: [
        {
          key: 0,
          label: "draft"
        },
        {
          key: 1,
          label: "scheduled"
        },
        {
          key: 2,
          label: "live"
        },
        {
          key: 3,
          label: "archived"
        }
      ],
      contentType: [
        "blog post",
        "workshop",
        "tweet",
        "website",
        "conference talk"
      ],
      contentTags: [
        "wildlife",
        "catnip",
        "adventures",
        "scratching posts",
        "javascript",
        "front end",
        "vue",
        "free lunch"
      ],
      selectedContentType: "",
      selectedContentStatus: "",
      selectedTags: [],
      searchTitle: "",
      searchAuthor: ""
    };
  },
  computed: {
    filteredContent() {
      let filteredContent = [...this.$options.contribContent];
      // First let's check for any tags we want
      // If no tags selected, the original array will be returned
      filteredContent = this.filteredContentByTags(
        filteredContent,
        this.selectedTags
      );
      // Now let's filter by title
      // If no title, then original array will be returned
      filteredContent = this.filteredContentByTitle(
        filteredContent,
        this.searchTitle
      );
      filteredContent = this.filteredContentByStatus(
        filteredContent,
        this.selectedContentStatus
      );
      filteredContent = this.filteredContentByType(
        filteredContent,
        this.selectedContentType
      );

      // Finally I will sort everything alphabetically by title
      // array.prototype.sort
      filteredContent.sort(function(a, b) {
        let titleA = a.title.toUpperCase();
        let titleB = b.title.toUpperCase();
        if (titleA < titleB) {
          return -1;
        }
        if (titleA > titleB) {
          return 1;
        }
        return 0;
      });

      return filteredContent;
    }
  },
  methods: {
    // These we don't need anymore, since we are using addRemoveTag method. Keeping for learning.
    // addTag(item) {
    //   if (this.selectedTags.indexOf(item) !== -1) {
    //     return;
    //   }
    //   this.selectedTags.push(item);
    // },
    // removeTag(item) {
    //   const index = this.selectedTags.indexOf(item);
    //   if (index === -1) return;
    //   this.selectedTags.splice(index, 1);
    // },
    addRemoveTag(tag) {
      if (this.selectedTags.indexOf(tag) !== -1) {
        const index = this.selectedTags.indexOf(tag);
        this.selectedTags.splice(index, 1);
      } else if (this.selectedTags.indexOf(tag) === -1) {
        this.selectedTags.push(tag);
      }
    },
    filteredContentByTags(content, tags) {
      // This will cpass us an array.
      // Check if no tags to filter by
      // Since we are just returning we don't need curly braces (but we could have them if we wanted)
      if (tags.length === 0) return content;
      // Now actually filter the content
      return content.filter(item => {
        if (!item.tags || item.tags.length === 0) return false;
        // Does item have the tag?
        for (let i = 0; i < tags.length; i++) {
          const tag = tags[i];
          // If we don't have ALL THE DAMN TAGS then the item won't show up on the screen.
          if (item.tags.indexOf(tag) === -1) return false;
        }
        // The item has all the tags selected, so we can finally return true
        return true;
      });
    },
    filteredContentByStatus(content, status) {
      if (!status) return content;
      return content.filter(item => {
        return item.content_status === status;
      });
    },
    filteredContentByTitle(content, title) {
      if (!title) return content;
      return content.filter(item => {
        return item.title.includes(title);
      });
    },
    filteredContentByType(content, type) {
      if (!type) return content;
      return content.filter(item => {
        return item.content_type.includes(type);
      });
    },
    // // Here I would like to click/unclick to sort ascending/descending by title
    // sortByTitle(content, title) {
    //   content.sort(function(a, b) {
    //     let titleA = a.title.toUpperCase();
    //     let titleB = b.title.toUpperCase();
    //     if (titleA < titleB) {
    //       return -1;
    //     }
    //     if (titleA > titleB) {
    //       return 1;
    //     }
    //     return 0;
    //   });
    // },
    statusNames(status) {
      if (status === 0) {
        return "draft";
      }
      if (status === 1) {
        return "scheduled";
      }
      if (status === 2) {
        return "live";
      }
      if (status === 3) {
        return "archived";
      }
      // If status is not any one of those, just return original value?
      return status;
    },
    tagClasses(tag) {
      const classes = [];
      if (this.selectedTags.includes(tag)) {
        classes.push("is-success");
      }
      return classes;
    }
  }
};
</script>
