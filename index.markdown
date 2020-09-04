---
layout: default
---
# How much does this debt really cost me?

> *"The most powerful force in the universe is compound interest"* - Albert Einstein probably didn't say this

When taking on any kind of loan, it is easy to overlook total amount you will end paying after interest. This is especially true with credit cards, where you are allowed to pay a minimum balance. As long as I pay the minimum amount each month, I'm fine, right? While paying the minimum amount will help you avoid late fees, it won't do much to reduce the amount you owe. In fact, you may find your debt growing, even if you aren't using any more credit.

The average **credit card debt** in the US is **$5,700**. With a minimum payment of $25 and a typical yearly interest rate of 23%, you will NEVER pay down your debt. After 5 years your debt will have grown to **$16,536.42**.

The average **student loan debt** is around **$35,000**. With a minimum payment of $50 and a typical yearly interest rate of 5.8%, you will NEVER pay down your debt. After 5 years your debt will have grown to **$46,271.67**.

Use the calculator below to find out how much you'll end up paying for your debt.

<form>
    <label for="balance">Outstanding Balance:</label>
    <input type="number" id="balance" name="balance" value="35000">

    <label for="payment">Monthly Payment:</label>
    <input type="number" id="payment" name="payment" value="50">

    <label for="interest">Yearly Interest Rate:</label>
    <input type="number" id="interest" name="interest" value="5.8"/>&nbsp;%
    
    <input type="submit" value="Calculate" onclick="calculate(event, this.form)"/>
</form>

<script>
    let node;
    
    function calculate(event, data) {
        event.preventDefault();
        if (node) {
            node.remove();
        }
        let monthly = parseFloat(data.payment.value);
        let initial_balance = balance = parseFloat(data.balance.value);
        let interest = ( (parseFloat(data.interest.value) / 12) / 100 )
        let new_balance = balance * (interest + 1);

        if (new_balance - balance >= monthly) {
            let ending_balance = balance + get_balance_after_months(12 * 5, balance, interest, monthly);

            let total_paid = monthly * 60;
            node = document.createElement("p")
            node.innerHTML = `Your balance is growing faster than your payment amount. You will never pay off your debt at this rate! After 5 years, after paying <b>$${total_paid}</b> in interest, your balance have grown to <b>$${ending_balance.toFixed(2)}</b>`
            document.body.appendChild(node);
            return;
        }

        let total_interest = 0;
        let months = 0;

        while (balance > 0) {
            let interest_amt = balance * interest;
            console.log(interest_amt)
            total_interest += interest_amt;
            balance = balance + interest_amt - monthly;
            months++
        }

        node = document.createElement("p")
        node.innerHTML =
            `<div>
                It will take you <b>${months}</b> months to pay off your balance.<br/>
                On top of the original balance of <b>$${initial_balance}</b>,
                you will pay an additional <i><b>$${total_interest.toFixed(2)}</b> in interest!</i>
            </div>`
        document.body.appendChild(node);
    }

    function get_balance_after_months(num_months, balance, interest, monthly) {
        let total_interest = 0;
        let months = 0;

        while (months < num_months) {
            let interest_amt = balance * interest;
            console.log(interest_amt)
            total_interest += interest_amt;
            balance = balance + interest_amt - monthly;
            months++
        }

        return total_interest;
    }
</script>