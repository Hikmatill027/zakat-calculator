<template>
    <div class="calculator-container">
        <div class="left-container">
            <h2>Zakat Calculator</h2>
            <form @submit.prevent="calculateZakat">
                <div class="form-group">
                    <label for="nisab">Nisab Threshold</label>
                    <input
                        type="number"
                        v-model="nisab"
                        placeholder="Enter Nisab amount"
                    />
                </div>
                <div class="form-group">
                    <label for="netIncome">Net Income</label>
                    <input
                        type="number"
                        v-model="netIncome"
                        placeholder="Enter Net Income"
                    />
                </div>
                <div class="form-group">
                    <label for="cashOnHand">Cash on Hand</label>
                    <input
                        type="number"
                        v-model="cashOnHand"
                        placeholder="Enter Cash on Hand"
                    />
                </div>
                <div class="form-group">
                    <label for="cashInBanks">Cash in Banks</label>
                    <input
                        type="number"
                        v-model="cashInBanks"
                        placeholder="Enter Cash in Banks"
                    />
                </div>
                <div class="form-group">
                    <label for="goldSilver">Gold and Silver</label>
                    <input
                        type="number"
                        v-model="goldSilver"
                        placeholder="Enter weight in grams"
                    />
                    <select v-model="metalType">
                        <option value="gold">Gold</option>
                        <option value="silver">Silver</option>
                    </select>
                </div>
                <button type="submit" class="calculate-button">
                    Calculate Zakat
                </button>
            </form>
        </div>

        <div class="right-container">
            <div v-if="zakatAmount === null" class="placeholder">
                <p>Results shown here</p>
                <p>
                    Complete the form and click "Calculate Zakat" to see your
                    Zakat amount.
                </p>
            </div>
            <div v-else class="result">
                <h3>Zakat Calculation Result</h3>
                <p>
                    Your Zakat amount is:
                    <strong
                        >{{ zakatAmount }} ({{
                            zakatAmount !== 0 ? "Payable" : "Not Payable"
                        }})</strong
                    >
                </p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            nisab: null,
            netIncome: null,
            cashOnHand: null,
            cashInBanks: null,
            goldSilver: null, // weight in grams
            metalType: "gold", // default metal type
            zakatAmount: null,
        };
    },
    methods: {
        calculateZakat() {
            // Define gold and silver prices per gram
            const goldPricePerGram = 60;
            const silverPricePerGram = 0.8;

            // Calculate the value of gold or silver in the selected weight
            const metalValue =
                this.metalType === "gold"
                    ? this.goldSilver * goldPricePerGram
                    : this.goldSilver * silverPricePerGram;

            // Calculate total assets
            const totalAssets =
                this.netIncome +
                this.cashOnHand +
                this.cashInBanks +
                metalValue;

            // Check if total assets exceed Nisab and calculate 2.5% of total assets if applicable
            this.zakatAmount =
                totalAssets > this.nisab ? (totalAssets * 0.025).toFixed(2) : 0;
        },
    },
};
</script>

<style scoped>
/* General layout */
.calculator-container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 20px;
    max-width: 900px;
    margin: 0 auto;
    background-color: #f0f4f8;
    border-radius: 8px;
}

/* Left container styles */
.left-container {
    width: 48%;
    padding: 20px;
    background-color: #ffffff;
    border-radius: 8px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.left-container h2 {
    margin-bottom: 20px;
}

.form-group {
    margin-bottom: 15px;
}

.form-group label {
    display: block;
    font-weight: bold;
    margin-bottom: 5px;
}

.form-group input,
.form-group select {
    width: 100%;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    margin-bottom: 5px;
}

.calculate-button {
    display: block;
    width: 100%;
    padding: 10px;
    background-color: #ffcc00;
    color: #000;
    border: none;
    border-radius: 4px;
    font-weight: bold;
    cursor: pointer;
}

.calculate-button:hover {
    background-color: #ffdd33;
}

/* Right container styles */
.right-container {
    width: 48%;
    padding: 20px;
    background-color: #1e2a38;
    border-radius: 8px;
    color: #ffffff;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
}

.placeholder {
    font-size: 1rem;
    color: #ffffff;
}

.result h3 {
    font-size: 1.5rem;
    margin-bottom: 10px;
}

/* Responsive styling */
@media (max-width: 768px) {
    .calculator-container {
        flex-direction: column;
    }

    .left-container,
    .right-container {
        width: 100%;
        margin-bottom: 20px;
    }
}
</style>
