## Mastering TDD and BDD in C#: From Novice to Expert
To become a high-level developer, you must move beyond simply "writing code that works" to "designing systems that are verifiable." This guide uses xUnit for testing and Fluent Assertions to create a BDD-style experience directly in C# code—no paid tools or complex Gherkin setups required.
------------------------------
## Phase 1: The Novice (Logic Mastery)
Project: FizzBuzz
Goal: Master the Red-Green-Refactor cycle.
Concept: Tests should be small and cover every branch of logic.

public class FizzBuzzTests
{
    [Theory]
    [InlineData(3, "Fizz")]
    [InlineData(5, "Buzz")]
    [InlineData(15, "FizzBuzz")]
    [InlineData(7, "7")]
    public void Generator_Should_Return_Correct_String(int input, string expected)
    {
        // Given (Arrange)
        var sut = new FizzBuzzGenerator();

        // When (Act)
        var result = sut.Generate(input);

        // Then (Assert)
        result.Should().Be(expected, "because the game rules dictate specific outputs for multiples of 3 and 5");
    }
}

------------------------------
## Phase 2: The Intermediate (State & Edge Cases)
Project: Simple Bank Account
Goal: Handle internal state and exceptions.
Concept: Use Fluent Assertions to verify that the system fails gracefully.

public class BankAccountTests
{
    [Fact]
    public void Withdraw_Should_Decrease_Balance_When_Funds_Are_Sufficient()
    {
        // Given
        var account = new BankAccount(initialBalance: 500);

        // When
        account.Withdraw(100);

        // Then
        account.Balance.Should().Be(400);
    }

    [Fact]
    public void Withdraw_Should_Throw_When_Funds_Are_Insufficient()
    {
        // Given
        var account = new BankAccount(100);

        // When
        Action act = () => account.Withdraw(150);

        // Then
        act.Should().Throw<InvalidOperationException>()
           .WithMessage("Insufficient Funds");
    }
}

------------------------------
## Phase 3: The Advanced (Dependency Isolation)
Project: Ticket Booking System
Goal: Isolate the system under test from external side effects (like emails).
Concept: Use Moq to verify interactions with dependencies.

public class BookingServiceTests
{
    [Fact]
    public void BookTicket_Should_Send_Email_When_Successful()
    {
        // Given
        var mailMock = new Mock<IEmailService>();
        var service = new BookingService(mailMock.Object);

        // When
        service.BookTicket("User@example.com", "Concert_A");

        // Then
        mailMock.Verify(m => m.SendEmail(It.IsAny<string>(), It.IsAny<string>()), Times.Once);
        // "Verify" ensures the code actually tried to send the email without needing a real mail server.
    }
}

------------------------------
## Phase 4: The Expert (The Architecture)
Project: E-commerce Checkout
Goal: Orchestrate multiple services and complex data.
Concept: Testing the "Integration" of components while keeping code decoupled.
At this level, you combine the previous three phases. You use Fluent Assertions to describe the business outcome, Moq to bypass third-party payment gateways, and TDD to build out the orchestration service that talks to your database repositories.
------------------------------
## Why use Fluent Assertions?

   1. Readability: result.Should().Be(10) reads like a sentence.
   2. Context: You can add .Because("reason") to explain why a test failed to future developers.
   3. No Gherkin overhead: You get the benefits of BDD's "Given/When/Then" structure without the hassle of maintaining separate .feature files and "glue code."


