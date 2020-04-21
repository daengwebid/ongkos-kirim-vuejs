<template>
    <div class="table-responsive">
        <table class="table table-hover table-bordered">
            <thead>
                <tr>
                    <th>Produk</th>
                    <th>Qty</th>
                    <th>Harga</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <th>Jaket Hoodie</th>
                    <td>1</td>
                    <td>Rp 250.000</td>
                </tr>
                <tr>
                    <th>Jaket Parka</th>
                    <td>2</td>
                    <td>Rp 250.000</td>
                </tr>
                <tr>
                    <th>Jaket Bomber</th>
                    <td>1</td>
                    <td>Rp 250.000</td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <th colspan="2">Subtotal</th>
                    <td>Rp {{ subtotal }}</td>
                </tr>
                <tr>
                    <th colspan="2">Ongkir</th>
                    <td>Rp {{ courier ? courier.cost:0 }}</td>
                </tr>
                <tr>
                    <th colspan="2">Total</th>
                    <td>Rp {{ total }}</td>
                </tr>
            </tfoot>
        </table>
        <div class="float-right mt-3">
            <button class="btn btn-primary btn-sm">Bayar!</button>
        </div>
    </div>
</template>

<script>
export default {
    props: ['courier'],
    data() {
        return {
            subtotal: 750000
        }
    },
    computed: {
        total() {
            let cost = this.courier ? this.courier.cost:0
            return parseFloat(this.subtotal) + parseFloat(cost)
        }
    }
}
</script>