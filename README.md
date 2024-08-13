//@version=5
indicator("9:42 AM Horizontal Line - Last 20 Days", overlay=true)

// Input parameters
line_color = input.color(color.blue, "9:42 AM Line Color")
line_width = input.int(2, "Line Width")
days_to_show = input.int(20, "Number of Days") // Set the number of days as an input

// Define the target time (9:42 AM)
target_hour_942 = 9
target_minute_942 = 42

// Function to calculate timestamp for a specific time in the specified timezone
get_target_timestamp(year, month, dayofmonth, hour, minute) =>
    timestamp("America/New_York", year, month, dayofmonth, hour, minute)

// Store the 9:42 AM price
var float nine_forty_two_price = na

// Check if the current candle is the 9:42 AM candle
if (hour(time) == target_hour_942 and minute(time) == target_minute_942)
    nine_forty_two_price := open

// Draw horizontal lines for the 9:42 AM price for the past number of days
if (not na(nine_forty_two_price))
    for i = 0 to days_to_show - 1
        // Calculate the target date for each day in the past
        target_time = get_target_timestamp(year, month, dayofmonth - i, target_hour_942, target_minute_942)
        
        // Find the bar index closest to the target time
        target_bar = ta.valuewhen(time >= target_time, bar_index, 0)
        
        // Ensure that the target_bar is valid
        if (not na(target_bar))
            // Draw horizontal line at the 9:42 AM price from the target_bar to the right edge of the chart
            line.new(x1=target_bar, y1=nine_forty_two_price, x2=bar_index, y2=nine_forty_two_price, color=line_color, width=line_width, xloc=xloc.bar_index, extend=extend.right)
