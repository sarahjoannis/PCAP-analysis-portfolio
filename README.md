# PCAP-analysis-portfolio
A detailed PCAP analysis and network forensics investigation of a file transfer from host p13, demonstrating packet-level analysis, HTTP inspection, and investigative reasoning.

---

# **PCAP Analysis Challenge – Network Forensics Investigation**

This project showcases my ability to perform **packet capture (PCAP) analysis**, investigate network behavior, and extract meaningful forensic conclusions from raw traffic data. The challenge involved analyzing network packets from **host p13** and identifying key details related to a file transfer over the network.

This repository includes:

* A detailed investigative report
* Screenshots of analysis steps
* Key insights and conclusions

---

## **Objectives**

The goal of this investigation was to identify:

1. IP address of the sender and receiver in network communication
2. IP address of the server
3. Name of the file sent through the network
4. Name of the web server that received the uploaded file
5. Directory where the file was uploaded
6. Total time taken to send the encrypted file

---

## **Tools and Techniques**

* **Wireshark** for filtering traffic, following streams, and inspecting protocol behavior
* **TCP stream analysis** to reconstruct communication and reassemble the uploaded file’s data
* **HTTP POST inspection** to identify file upload details, including filename, server, and directory
* **Packet timestamps** to calculate file transfer duration

---

## **Investigation Steps**

### **1. Identifying Sender and Receiver IP Addresses**

* Filtered traffic associated with host p13 using:

```
tcp contains "P13"
```

* Identified IP addresses:

```
Source IP: 192.168.235.137  
Destination IP: 192.168.235.131
```
[description of image](https://github.com/sarahjoannis/PCAP-analysis-portfolio/commit/869e6942ab8c8e17877a778a1f1b6483b7bad2ea#diff-3e3f90c1c99e59905c1ded146aa3e86ed3d4a45731b18d0fa63f1e3bba3fe83c)*

### **2. Identifying the Server IP Address**

* Filtered for HTTP POST requests:

```
http.request.method == "POST"
```

* Examined the Internet Protocol layer in the relevant packet
* Server IP found: `192.168.1.7`

*Insert screenshot here*

---

### **3. Determining the File Name**

* Used the same HTTP POST filter
* Expanded:

```
MIME → Multipart Media Encapsulation → Multipart Encapsulated Part
```

* Found file name under **Content-Disposition**:

```
Filename: file
```

*Insert screenshot here*

---

### **4. Identifying the Web Server**

* Filtered for HTTP responses with `HTTP/1.1 200 OK`
* Examined the **Hypertext Transfer Protocol** header
* Server name: **Apache**

*Insert screenshot here*

---

### **5. Determining the Upload Directory**

* Expanded **Line-based Text Data** in the POST request
* Observed:

```
File uploaded at uploads/file
```

* Upload directory: **/uploads/**

*Insert screenshot here*

---

---

### **6. Calculating Upload Duration (Using Statistics Conversion)**

1. **Go to Statistics** → **Conversion** → **TCP Stream**.
2. **Identify the stream**:

   * Filter for the **source** and **destination IP** involved in the file transfer (based on the IP addresses you identified earlier).
   * Look for the **TCP stream** that contains the file payload, ensuring that the **byte size** corresponds to the file transfer size.
3. **View the duration**:

   * In the **TCP Stream** window, Wireshark will show the **duration** of the transfer along with the **bytes transferred**.
4. **Confirm the stream**:

   * By matching the **payload size** and the **source/destination IPs**, you can confirm you're looking at the correct stream.
5. **Record the total duration**:

   * The total upload duration was **0.0073 seconds**.

*Insert screenshot here*

---

---

## **Summary of Findings**

| **Item**             | **Result**                          |
| -------------------- | ----------------------------------- |
| **Sender IP**        | 192.168.235.137                     |
| **Receiver IP**      | 192.168.235.131                     |
| **Server IP**        | 192.168.1.7                         |
| **File Name**        | file                                |
| **Web Server**       | Apache                              |
| **Upload Directory** | /uploads/                           |
| **Upload Duration**  | 0.0073 seconds                      |

---

## **Lessons Learned**

Working through this PCAP analysis challenge was a rewarding experience that reminded me how powerful and fascinating network forensics can be. Starting from raw packets and piecing together the entire story — from identifying IP addresses to uncovering the file name, upload directory, and timing — felt like solving a real digital mystery.

This process deepened my appreciation for the details hidden in network traffic and strengthened my investigative mindset. It reinforced the importance of patience and careful observation when interpreting data that, at first glance, might seem like noise.

For me, this project was a great reminder to trust the process, follow the clues, and never underestimate the value of a methodical approach.

---

