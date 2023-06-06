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
          <select @change="$event => update_parent($event, 0, 1)">
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
        <div class="trait-selector">
          <h3>Trait 2</h3>
          <select @change="$event => update_parent($event, 1, 1)">
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
        <div class="trait-selector">
          <h3>Trait 3</h3>
          <select @change="$event => update_parent($event, 2, 1)">
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
      </div>
      <div id="secondaryParent" class="parent">
        <h1>Secondary Parent</h1>
        <div class="trait-selector">
          <h3>Trait 1</h3>
          <select @change="$event => update_parent($event, 0, 2)">
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
        <div class="trait-selector">
          <h3>Trait 2</h3>
          <select @change="$event => update_parent($event, 1, 2)">
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
        <div class="trait-selector">
          <h3>Trait 3</h3>
          <select @change="$event => update_parent($event, 1, 2)">
            <option v-for="trait in traits" v-bind:value="trait.name">{{ trait.name }}</option>
          </select>
        </div>
      </div>
      <button @click="generate_child()">Generate Child</button>
    </div>
    <div v-show="show_child" id="child">
      <h2>Child</h2>
      <div class="child-trait">
        <h3>Infant Trait:</h3>
        <h3>{{ infant_trait }}</h3>
      </div>
      <div class="child-trait">
        <h3>Toddler Trait:</h3>
        <h3>{{ toddler_trait }}</h3>
      </div>
      <div class="child-trait">
        <h3>Child Trait:</h3>
        <h3>{{ child_trait }}</h3>
      </div>
      <div class="child-trait">
        <h3>Teen Trait:</h3>
        <h3>{{ teen_trait }}</h3>
      </div>
      <div class="child-trait">
        <h3>Adult Trait:</h3>
        <h3>{{ adult_trait }}</h3>
      </div>
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
                   {name: 'Neat', dlc: 0}, {name: 'Noncommital', dlc: 0}, {name: 'Outgoing', dlc: 0}, {name: 'Overachiever', dlc: 9}, {name: 'Paranoid', dlc: 11}, {name: 'Party Animal', dlc: 9}, {name: 'Perfectionist', dlc: 0},
                   {name: 'Proper', dlc: 4}, {name: 'Recycle Disciple', dlc: 3}, {name: 'Romantic', dlc: 0}, {name: 'Self-Absorbed', dlc: 12}, {name: 'Self-Assured', dlc: 0}, {name: 'Slob', dlc: 0}, {name: 'Snob', dlc: 0}, 
                   {name: 'Socially Awkward', dlc: 9}, {name: 'Squeamish', dlc: 10}, {name: 'Unflirty', dlc: 2}, {name: 'Vegetarian', dlc: 0}]
        return {
          dlcPerRow: 5,
          dlcs: [{id: 0, name: 'Base'}, {id: 1, name: 'Spa Day'}, {id: 2, name: 'City Living'}, {id: 3, name: 'Eco Lifestyle'}, {id: 4, name: 'Snowy Escape'}, {id: 5, name: 'Cottage Living'},
                 {id: 6, name: 'Cats & Dogs'}, {id: 7, name: 'Island Living'}, {id: 8, name: 'Get Together'}, {id: 9, name: 'High School Years'}, {id: 10, name: 'Outdoor Retreat'}, 
                 {id: 11, name: 'Strangerville'}, {id: 12, name: 'Get Famous'}],
          all_traits: defaultTraits,
          traits: defaultTraits,
          show_child: false,
          infant_trait: "infant",
          toddler_trait: "toddler",
          child_trait: "child",
          teen_trait: "teen",
          adult_trait: "adult",
          parent_1_traits: [defaultTraits[0].name, defaultTraits[0].name, defaultTraits[0].name],
          parent_2_traits: [defaultTraits[0].name, defaultTraits[0].name, defaultTraits[0].name]
        }
    },
    computed: {
      dlcRows() {
        return Math.ceil(this.dlcs.length / this.dlcPerRow);
      },
      parent_traits() {
        let parent_traits = []
        this.parent_1_traits.forEach(trait => parent_traits.push(trait))
        this.parent_2_traits.forEach(trait => parent_traits.push(trait))
        
        return parent_traits
      }
    },
    methods: {
      set_traits(event) {
        let dlc_id = parseInt(event.currentTarget.id)
        
        if(event.currentTarget.checked) {
          this.all_traits.forEach(trait => {
            if(trait.dlc == dlc_id) {
              this.traits.push(trait)
            }
          })
        }
        else {
          let temp_traits = []
          this.traits.forEach(trait => {
            if(trait.dlc != dlc_id) {
              temp_traits.push(trait)
            }
          })
          this.traits = temp_traits
        }

        this.sort_traits()
      },
      sort_traits() {
        this.traits.sort((a, b) => (a.name > b.name) ? 1 : -1)
      },
      update_parent(event, trait_num, parent) {
        if(parent == 1)
          this.parent_1_traits[trait_num] = event.target.value
        else 
          this.parent_2_traits[trait_num] = event.target.value
      },
      generate_child() {
        this.pick_random_infant_trait()
        this.pick_random_toddler_trait()

        let num_inherited_traits = 0

        this.child_trait = this.generate_trait(num_inherited_traits)
        if(this.parent_traits.includes(this.child_trait))
            num_inherited_traits++

        do {
          this.teen_trait = this.generate_trait(num_inherited_traits)
        } while(this.child_trait == this.teen_trait)

        if(this.parent_traits.includes(this.teen_trait))
            num_inherited_traits++

        do {
          this.adult_trait = this.generate_trait(num_inherited_traits)
        } while(this.child_trait == this.adult_trait || this.teen_trait == this.adult_trait)

        this.show_child = true
      },
      pick_random_infant_trait() {
        let infant_traits = ["Cautious", "Sensitive", "Calm", "Intense", "Wiggly", "Sunny"]
        this.infant_trait = infant_traits[this.get_random_int(infant_traits.length)]
      },
      pick_random_toddler_trait() {
        let toddler_traits = ["Angelic", "Charmer", "Clingy", "Fussy", "Independant", "Inquisitive", "Silly", "Wild"]
        this.toddler_trait = toddler_traits[this.get_random_int(toddler_traits.length)]
      },
      get_random_int(max) {
        return Math.floor(Math.random() * max)
      },
      should_inherit_trait(num_inherited_traits) {
        let num = this.get_random_int(10)
        console.log(`num inherited: ${num_inherited_traits} new: ${num}`)

        if(num_inherited_traits == 0) 
          return num < 5
        else if(num_inherited_traits == 1)
          return num < 3
        else 
          return num <= 1
      },
      generate_trait(num_inherited_traits) {
        console.log('generate')
        let inherit = this.should_inherit_trait(num_inherited_traits)
        console.log(`inherit: ${inherit}`)

        if(inherit) {
          return this.parent_traits[this.get_random_int(this.parent_traits.length)]
        } else {
          return this.traits[this.get_random_int(this.traits.length)].name
        }
      }
    }
}
</script>