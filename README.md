# Network-Usage-Monitor-and-Pie-Chart-Generator
import psutil
import matplotlib.pyplot as plt

# Function to get network usage
def get_network_usage():
    network_stats = psutil.net_io_counters()
    return network_stats.bytes_sent, network_stats.bytes_recv, network_stats.packets_sent, network_stats.packets_recv

# Function to display network usage as a pie chart
def display_network_usage_pie_chart(sent, received):
    labels = 'Sent', 'Received'
    sizes = [sent, received]
    colors = ['blue', 'green']
    explode = (0.1, 0)  # explode the 1st slice (Sent)

    plt.pie(sizes, explode=explode, labels=labels, colors=colors, autopct='%1.1f%%', shadow=True, startangle=140)
    plt.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.
    plt.title('Network Usage')
    plt.show()

if __name__ == "__main__":
    try:
        sent_before, received_before, packets_sent_before, packets_received_before = get_network_usage()
        input("Press Enter to stop monitoring...")
        sent_after, received_after, packets_sent_after, packets_received_after = get_network_usage()

        sent_diff = sent_after - sent_before
        received_diff = received_after - received_before
        packets_sent_diff = packets_sent_after - packets_sent_before
        packets_received_diff = packets_received_after - packets_received_before

        print(f"Sent Bytes: {sent_diff}")
        print(f"Received Bytes: {received_diff}")
        print(f"Sent Packets: {packets_sent_diff}")
        print(f"Received Packets: {packets_received_diff}")

        display_network_usage_pie_chart(sent_diff, received_diff)
    except KeyboardInterrupt:
        print("\nMonitoring stopped.")
