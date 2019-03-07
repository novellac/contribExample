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
              v-model="searchTitle"
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
                <option v-for="item in contentStatus" v-bind:key="item.key">{{
                  item.label
                }}</option>
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
              v-for="(item, index) in contentTags"
              v-bind:key="index"
              class="control"
            >
              <div class="tags has-addons">
                <a class="tag is-link is-medium">{{ item }}</a>
                <a class="tag is-medium is-delete"></a>
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
        <tr
          v-for="(tablerow, index) in $options.contribContent"
          v-bind:key="index"
        >
          <td>
            <p>
              {{ tablerow.content_status
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
                <a>{{ tag }}</a>
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
    <h2>Goals/Questions</h2>
    <ul>
      <li>
        Assign names to content status numbers (maybe use a computed property?)
      </li>
      <li>
        Make tags list gather all possible tags from json (do not hard code in
        data)
      </li>
      <li>
        Click on multiple tags to show items which have all selected terms.
      </li>
      <li>Click on tag's "x" to remove from filters.</li>
      <li>
        Filter by publication status by clicking on a publication status or by
        using dropdown
      </li>
      <li>
        Sort by sort by publication date by clicking "status" in table header
      </li>
      <li>Sort by created by clicking on "created" in the table header</li>
      <li>Sort by created by clicking on "updated" in the table header</li>
      <li>Search by title using search input</li>
      <li>
        Search by author's name or author's twitter handle using search input
      </li>
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
  }
};
</script>
