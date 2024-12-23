# Nmap-Web-Scanner
import nmap

def scan_target(target):
    """Scans a single target using Nmap."""
    try:
        scanner = nmap.PortScanner()
        scanner.scan(hosts=target, arguments="-sV -sC") 
        # -sV: Service version detection
        # -sC: Script scanning
        for host in scanner.all_hosts():
            print("-" * 50)
            print("Host: %s (%s)" % (host, scanner[host].hostname()))
            print("State: %s" % scanner[host].state())
            for proto in scanner[host].all_protocols():
                print("Protocol: %s" % proto)
                lport = scanner[host][proto].keys()
                for port in lport:
                    print("port : %s\tstate : %s" % (port, scanner[host][proto][port]['state']))
    except Exception as e:
        print(f"Error scanning {target}: {e}")

if __name__ == "__main__":
    with open("targets.txt", "r") as f:
        targets = f.read().splitlines()

    for target in targets:
        scan_target(target)
        
