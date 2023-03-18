# Payment method integrated with multiple payment gateway
- using Factory and Strategy pattern
- Factory to create object when to use paypay, waller or stripe
- Strategy pattern to execute the spcific payment method
# RUn order steps using Chain repsonsibility pattern
```c#
public class Order
{
    public int OrderId { get; set; }
    public string OrderType { get; set; }
    public decimal Amount { get; set; }
    public string PaymentMethod { get; set; }
}

public abstract class OrderStep
{
    protected OrderStep _nextStep;

    public OrderStep SetNextStep(OrderStep step)
    {
        _nextStep = step;
        return step;
    }

    public abstract bool CanHandle(Order order);
    public abstract void Execute(Order order);
}

public class SaveOrderStep : OrderStep
{
    public override bool CanHandle(Order order)
    {
        return order.OrderType == "buy" || order.OrderType == "sell";
    }

    public override void Execute(Order order)
    {
        Console.WriteLine("Order saved successfully");
        _nextStep?.Execute(order);
    }
}

public class AddHoldingPiesStep : OrderStep
{
    public override bool CanHandle(Order order)
    {
        return order.OrderType == "buy";
    }

    public override void Execute(Order order)
    {
        Console.WriteLine("Holding pies added successfully");
        _nextStep?.Execute(order);
    }
}

public class UpdateWalletBalanceStep : OrderStep
{
    public override bool CanHandle(Order order)
    {
        return order.PaymentMethod == "wallet";
    }

    public override void Execute(Order order)
    {
        Console.WriteLine("Wallet balance updated successfully");
        _nextStep?.Execute(order);
    }
}

public class ReduceAmountStep : OrderStep
{
    public override bool CanHandle(Order order)
    {
        return order.OrderType == "buy" || order.OrderType == "sell";
    }

    public override void Execute(Order order)
    {
        Console.WriteLine("Amount reduced successfully");
        _nextStep?.Execute(order);
    }
}

public class UpdateOrderStatusStep : OrderStep
{
    public override bool CanHandle(Order order)
    {
        return true;
    }

    public override void Execute(Order order)
    {
        Console.WriteLine("Order status updated successfully");
        _nextStep?.Execute(order);
    }
}

public class CreateTransactionHistoryStep : OrderStep
{
    public override bool CanHandle(Order order)
    {
        return true;
    }

    public override void Execute(Order order)
    {
        Console.WriteLine("Transaction history created successfully");
    }
}


// Example usage
Order order = new Order { OrderId = 1, OrderType = "buy", Amount = 100, PaymentMethod = "wallet" };

SaveOrderStep saveOrderStep = new SaveOrderStep();
AddHoldingPiesStep addHoldingPiesStep = new AddHoldingPiesStep();
UpdateWalletBalanceStep updateWalletBalanceStep = new UpdateWalletBalanceStep();
ReduceAmountStep reduceAmountStep = new ReduceAmountStep();
UpdateOrderStatusStep updateOrderStatusStep = new UpdateOrderStatusStep();
CreateTransactionHistoryStep createTransactionHistoryStep = new CreateTransactionHistoryStep();

saveOrderStep.SetNextStep(addHoldingPiesStep).SetNextStep(updateWalletBalanceStep)
    .SetNextStep(reduceAmountStep).SetNextStep(updateOrderStatusStep)
    .SetNextStep(createTransactionHistoryStep);

saveOrderStep.Execute(order);

```