<template>
  <div id="container">
    <div id="banner" class="row green">
      <h1>Sims Trait Generator</h1>
    </div>
    <div id="dlcs"  style="display: flex; flex-direction: column;" class="row">
      <div v-for="row in dlcRows" :key="row" style="display: flex; flex-direction: row;">
        <div v-for="col in dlcPerRow" :key="col" :set="index=(row - 1) * dlcPerRow + col - 1">
          <label v-if="index < dlcs.length" v-bind:for="dlcs[index].id"> 
            <input type="checkbox" v-bind:id="dlcs[index].id" v-bind:name="dlcs[index].name" v-bind:value="dlcs[index].name" @change="set_traits($event)" checked>
            {{ dlcs[index].name }}
          </label>
        </div>
      </div>
    </div>
    <div id="traits" class="row">
      <div id="primaryParent" class="parent">
        <h1>Primary Parent</h1>
        <div class="trait-selector">
          <h3>Trait 1</h3>
          <select>
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
        <div class="trait-selector">
          <h3>Trait 2</h3>
          <select>
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
        <div class="trait-selector">
          <h3>Trait 3</h3>
          <select>
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
      </div>
      <div id="secondaryParent" class="parent">
        <h1>Secondary Parent</h1>
        <div class="trait-selector">
          <h3>Trait 1</h3>
          <select>
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
        <div class="trait-selector">
          <h3>Trait 2</h3>
          <select>
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
        <div class="trait-selector">
          <h3>Trait 3</h3>
          <select>
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
      </div>
      <button>Generate Child</button>
    </div>
  </div>
</template>
<script>
export default {
  data() { 
        let defaultTraits = [{name: 'Active', dlc: 0}, {name: 'Adventurous', dlc: 4}, {name: 'Ambitious', dlc: 0}, {name: 'Animal Enthusiast', dlc: 5}, {name: 'Art Lover', dlc: 0}, {name: 'Bookworm', dlc: 0}, {name: 'Bro', dlc: 0},
                   {name: 'Cat Lover', dlc: 6}, {name: 'Cheerful', dlc: 0}, {name: 'Child of the Islands', dlc: 7}, {name: 'Child of the Oceans', dlc: 7}, {name: 'Childish', dlc: 0}, {name: 'Clumsy', dlc: 0},
                   {name: 'Creative', dlc: 0}, {name: 'Foodie', dlc: 0}, {name: 'Dance Machine', dlc: 8}, {name: 'Dog Lover', dlc: 6}, {name: 'Erratic', dlc: 0}, {name: 'Evil', dlc: 0}, {name: 'Family-Oriented', dlc: 0}, 
                   {name: 'Freegan', dlc: 3}, {name: 'Geek', dlc: 0}, {name: 'Genius', dlc: 0}, {name: 'Gloomy', dlc: 0}, {name: 'Glutton', dlc: 0}, {name: 'Good', dlc: 0}, {name: 'Goofball', dlc: 0}, {name: 'Green Fiend', dlc: 3}, 
                   {name: 'Hates Children', dlc: 0}, {name: 'High Maintenance', dlc: 1}, {name: 'Hot-Headed', dlc: 0}, {name: 'Insider', dlc: 8}, {name: 'Jealous', dlc: 0}, {name: 'Kleptomaniac', dlc: 0}, {name: 'Lactose Intolerant', dlc: 5}, 
                   {name: 'Lazy', dlc: 0}, {name: 'Loner', dlc: 0}, {name: 'Loves Outdoors', dlc: 0}, {name: 'Loyal', dlc: 0}, {name: 'Maker', dlc: 3}, {name: 'Materialistic', dlc: 0}, {name: 'Mean', dlc: 0}, {name: 'Music Lover', dlc: 0}, 
                   {name: 'Neat', dlc: 0}, {name: 'Noncommital', dlc: 0}, {name: 'Outgoing', dlc: 0}, {name: 'Overachiever', dlc: 9}, {name: 'Paramoid', dlc: 11}, {name: 'Party Animal', dlc: 9}, {name: 'Perfectionist', dlc: 0},
                   {name: 'Proper', dlc: 4}, {name: 'Recycle Disciple', dlc: 3}, {name: 'Romantic', dlc: 0}, {name: 'Self-Absorbed', dlc: 12}, {name: 'Self-Assured', dlc: 0}, {name: 'Slob', dlc: 0}, {name: 'Snob', dlc: 0}, 
                   {name: 'Socially Awkward', dlc: 9}, {name: 'Squeamish', dlc: 10}, {name: 'Unflirty', dlc: 2}, {name: 'Vegetarian', dlc: 0}]
        return {
          dlcPerRow: 5,
          dlcs: [{id: 0, name: 'Base'}, {id: 1, name: 'Spa Day'}, {id: 2, name: 'City Living'}, {id: 3, name: 'Eco Lifestyle'}, {id: 4, name: 'Snowy Escape'}, {id: 5, name: 'Cottage Living'},
                 {id: 6, name: 'Cats & Dogs'}, {id: 7, name: 'Island Living'}, {id: 8, name: 'Get Together'}, {id: 9, name: 'High School Years'}, {id: 10, name: 'Outdoor Retreat'}, 
                 {id: 11, name: 'Strangerville'}, {id: 12, name: 'Get Famous'}],
          all_traits: defaultTraits,
          traits: defaultTraits
        }
    },
    computed: {
      dlcRows() {
        return Math.ceil(this.dlcs.length / this.dlcPerRow);
      }
    },
    methods: {
      set_traits(event) {
        let dlc_id = parseInt(event.currentTarget.id)
        
        if(event.currentTarget.checked) {
          console.log("checked")
          this.all_traits.forEach(trait => {
            if(trait.dlc == dlc_id) {
              console.log(`add ${trait}`)
              this.traits.push(trait)
            }
          })
        }
        else {
          let temp_traits = []
          this.traits.forEach(trait => {
            if(trait.dlc != dlc_id) {
              temp_traits.push(trait)
              console.log(`add ${trait}`)
            }
          })
          this.traits = temp_traits
        }

        this.sort_traits()
      },
      sort_traits() {
        this.traits.sort((a, b) => (a.name > b.name) ? 1 : -1)
      }
    }
}
</script>