# Convert-number-into-a-comma-separated-indian-currency-format
def format_indian_currency(number):
    """
    Convert a number into Indian currency format with commas.
    
    Args:
        number: The number to be formatted (int or float)
    
    Returns:
        String representation of the number in Indian format
    """
    # Handle negative numbers
    sign = '-' if number < 0 else ''
    number = abs(number)
    
    # Convert to string and split into integer and fractional parts
    num_str = "{:.2f}".format(number)
    integer_part, fractional_part = num_str.split('.')
    
    # Process the integer part
    length = len(integer_part)
    
    if length <= 3:
        formatted_integer = integer_part
    else:
        last_three = integer_part[-3:]
        other_digits = integer_part[:-3]
        
        # Add commas after every 2 digits except the first group
        formatted_other = []
        while len(other_digits) > 0:
            if len(other_digits) == 1:  # For 1 digit remaining (like 1 in 1,00,000)
                formatted_other.insert(0, other_digits[-1:])
                other_digits = other_digits[:-1]
            else:
                formatted_other.insert(0, other_digits[-2:])
                other_digits = other_digits[:-2]
        
        formatted_integer = ','.join(formatted_other) + ',' + last_three
    
    # Combine all parts
    formatted_number = f"{sign}{formatted_integer}.{fractional_part}"
    
    # Remove trailing .00 if it's an integer
    if fractional_part == '00':
        formatted_number = formatted_number[:-3]
    
    return formatted_number

# Example usage
if __name__ == "__main__":
    numbers = [
        1,
        12,
        123,
        1234,
        12345,
        123456,
        1234567,
        12345678,
        123456789,
        1234567890,
        12345678901,
        123456789012,
        10000000,  # 1 crore
        100000,    # 1 lakh
        100000000,  # 10 crore
        -12345678,
        1234.56,
        123456.78,
        12345678.90
    ]
    
    for num in numbers:
        print(f"{num:15} => {format_indian_currency(num)}")
