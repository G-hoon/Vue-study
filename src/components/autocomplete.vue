<template>
    <div class="autocomplete"
         v-on:keyup="keyPress($event)">
        <div class="input-box">
            <input v-model="autocompleteValue" 
                   v-on:focus="activated = true"
                   v-on:blur="activated = false"
                   v-on:keyup="changeValue(autocompleteValue)">
        </div>
        <div class="option-panel" v-if="activated">
            <ul>
                <li class="option-item" :class="itemIndex === index ? 'selected-item' : ''" 
                    v-for="(item, index) in autocompleteCandidate" :key="`item-${index}`" 
                    v-on:mousedown="onClickItem(item)"
                    v-on:mouseover="onMouseOver(index)"
                    v-on:mouseout="onMouseOut()">{{item}}</li>
            </ul>
        </div>
    {{activated}} / {{itemIndex}}
    </div>
</template>
<script>
function filteredListForPartialMatch(arr, sub) {
    sub = sub.toLowerCase();
    return arr.filter(str => sub.length <= str.length && 
                             str.toLowerCase()
                                .startsWith(sub.slice(0, str.length)));
}

export default {
    name: 'CwAutocomplete',
    data: function () {
        return {
            activated: false,
            autocompleteCandidate: this.list,
            autocompleteValue: this.model,
            backupAutocompleteCandidate: [],
            itemIndex: -1,
        }
    },
    props: {
        list: {
            type: Array,
            default: () => ([])
        },
        model: {
            type: String,
            default: () => ('')
        }
    },
    computed: {
        
    },
    methods: {
        changeValue: function (item) {
            if (item.length > 0) {
                this.$data.autocompleteCandidate = filteredListForPartialMatch(this.$data.backupAutocompleteCandidate, item);
            }
            else {
                this.$data.autocompleteCandidate =  this.$data.backupAutocompleteCandidate.slice(0);
            }
            
        },
        onClickItem: function (item) {
            this.$data.autocompleteValue = item;
            this.changeValue(item);
            this.itemIndex = -1;
        },
        keyPress: function (key) {
            if (key.code === 'Enter') {
                if (this.itemIndex >= 0) {
                    this.$data.autocompleteValue = this.autocompleteCandidate[this.itemIndex];
                    this.changeValue(this.autocompleteCandidate[this.itemIndex]);
                    this.itemIndex = -1;
                }
            }
            if (key.code === 'ArrowUp') {
                if (this.itemIndex <= -1) {
                    return;
                }
                this.itemIndex--;
            }
            if (key.code === 'ArrowDown') {
                if (this.itemIndex >= (this.autocompleteCandidate.length - 1)) {
                    this.itemIndex = 0;
                }
                else {
                    this.itemIndex++;
                }
            }
        },
        onMouseOver(index) {
            this.itemIndex = index;
        },
        onMouseOut() {
            this.itemIndex = -1;
        }
    },
    created () {
        this.$data.backupAutocompleteCandidate = this.$data.autocompleteCandidate.slice(0);
    }
}
</script>

<style>
.autocomplete {
    width: 500px;
}
input {
    width: 100%;
}
ul {
    margin: 0px;
    padding: 10px;
    list-style-type: none;
}
.option-panel {
    background-color: whitesmoke;
    border-style: solid;
    border-width: 1px;
    border-color: gray;
}

.option-item {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    display:block;
}

.selected-item {
    font-weight: bold;
    color: brown;
    background-color:wheat;
}

</style>
