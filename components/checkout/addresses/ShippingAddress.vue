<template>
    <article class="message is-dark">
        <div class="message-body">
        <h1 class="title is-5">Ship to</h1>
            
            <template v-if="selecting">
                <ShippingAddressSelector
                    :addresses="addresses"
                    :selectedAddress="selectedAddress"
                    @click="addressSelected"
                />
            </template>

            <template v-else-if="creating">
                <ShippingAddressCreator @cancel="creating = false" @created="created" />
            </template>

            <template v-else>
                <template v-if="selectedAddress">
                    <p>
                        {{ selectedAddress.name }} <br>
                        {{ selectedAddress.address_1 }} <br>
                        {{ selectedAddress.city }} <br>
                        {{ selectedAddress.postal_code }}<br>
                        {{ selectedAddress.country.name }} <br>
                    </p>
                </template>
                <br>
                <div class="field is-grouped">
                    <p class="control">
                        <a href="" class="button is-link is-outlined is-size-6" @click.prevent="selecting = true">
                            Change Address
                        </a>
                    </p>

                    <p class="control">
                        <a href="" class="button is-link is-outlined is-size-6" @click.prevent="creating = true">
                            New Address
                        </a>
                    </p>
                </div>
            </template>
        </div>
    </article>
</template>

<script>
    import ShippingAddressSelector from './ShippingAddressSelector'
    import ShippingAddressCreator from './ShippingAddressCreator'

    export default {
        data() {
            return {
                selecting: false,
                creating: false, 
                localAddresses: this.addresses,
                selectedAddress: null
            }
        },

        components: {
            ShippingAddressSelector,
            ShippingAddressCreator
        },
        
        props: {
            addresses: {
                required: true,
                type: Array
            }
        },

        computed: {
            defaultAddress() {
                return this.localAddresses.find(a => a.default === 1)
            }
        },

        methods: {
            addressSelected(address) {
                this.switchAddress(address)
                this.selecting = false
            },

            switchAddress(address) {
                this.selectedAddress = address
            },

            created(address) {
                this.localAddresses.push(address)
                this.creating = false
                this.switchAddress(address)
            }
        },

        created() {
            if (this.addresses.length) {
                this.switchAddress(this.defaultAddress)
            }
        }
    }
</script>