# 🛡️ Container Security & Linux Kernel (eBPF) Research

> **Project Scope:** From Bachelor's Thesis to Master's Research  
> **Domain:** DevSecOps | Cloud Security | Linux Kernel | Red Teaming

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![Go](https://img.shields.io/badge/go-%2300ADD8.svg?style=for-the-badge&logo=go&logoColor=white)

## 📖 Giới thiệu (Introduction)

Dự án này tập trung nghiên cứu vùng giao thoa giữa **DevOps** (Kubernetes/Docker), **System** (Linux Kernel) và **Security**. Đây là kho lưu trữ tài liệu, mã nguồn và các kịch bản thực nghiệm cho lộ trình nghiên cứu dài hạn về bảo mật Container.

Mục tiêu của dự án là đi từ việc hiểu cơ chế tấn công/phòng thủ ở tầng ứng dụng (User-space) đến việc xây dựng các giải pháp bảo mật chuyên sâu ở tầng nhân (Kernel-space) sử dụng công nghệ eBPF.

---

## 🎓 Giai đoạn 1: Đồ án Tốt nghiệp Đại học (Bachelor's Thesis)

**Chủ đề:** *Nghiên cứu các kỹ thuật tấn công thoát ly (Container Breakout) trong môi trường Kubernetes và giải pháp phòng chống.*

Trong giai đoạn này, dự án tập trung vào việc xây dựng môi trường Lab Kubernetes chứa các lỗ hổng cấu hình phổ biến (Misconfigurations) và thực hiện Pentest/Hardening.

### 🔴 Red Team: Kịch bản Tấn công (Attack Vectors)
Các kỹ thuật tấn công thoát ly (Container Breakout) được mô phỏng:

1.  **Insecure Capabilities:**
    * Khai thác Container được cấp quyền `SYS_ADMIN`.
    * Khai thác Container chạy ở chế độ `privileged`.
2.  **Mounting Sensitive Paths:**
    * Leo thang đặc quyền thông qua việc mount `/var/run/docker.sock`.
    * Chiếm quyền Host thông qua việc mount thư mục `/etc` hoặc `/`.
3.  **Kernel Exploits:**
    * Sử dụng các lỗ hổng nhân Linux cũ (ví dụ: Dirty COW) để leo thang từ Container ra Host.

### 🔵 Blue Team: Giải pháp Phòng thủ (Defense Strategies)
Triển khai các biện pháp Hardening cho cụm K8s:

1.  **Pod Security Standards (PSS):** Áp dụng các tiêu chuẩn để chặn Pod không an toàn.
2.  **Policy-as-Code:** Sử dụng **Kyverno** hoặc **OPA Gatekeeper** để tự động chặn việc tạo Container có quyền root hoặc mount hostPath nhạy cảm.
3.  **Image Scanning:** Tích hợp **Trivy** để quét lỗ hổng trong Docker Image trước khi deploy.

### 🛠️ Công nghệ sử dụng (Tech Stack)
* **Infrastructure:** Minikube / Kind, Docker.
* **Attack Tools:** `kubectl`, `amicontainer`, `peirates`.
* **Defense Tools:** Kyverno, Trivy.

---

## 🎓 Giai đoạn 2: Luận văn Thạc sĩ (Master's Thesis)

**Chủ đề:** *Phát triển hệ thống giám sát và ngăn chặn tấn công Runtime cho Container dựa trên công nghệ eBPF.*

Giai đoạn này đi sâu vào **Linux Kernel** để giải quyết vấn đề hiệu năng và khả năng ẩn mình của các công cụ bảo mật truyền thống.

### 🔬 Vấn đề nghiên cứu (Research Gap)
* Antivirus truyền thống chạy ở User-space dễ bị Hacker tắt (Kill process) hoặc bị bypass.
* Các giải pháp cũ thường làm chậm hệ thống (Performance overhead).
* **Giải pháp:** Sử dụng **eBPF** để chạy code Sandbox an toàn ngay trong nhân Linux, cho phép giám sát toàn diện với hiệu năng cực cao.

### 💻 Nội dung thực hiện (Implementation)
Phát triển chương trình eBPF (C/Go/Rust) với các chức năng:

1.  **Syscall Hooking:**
    * Hook vào `execve`: Giám sát lệnh được thực thi.
    * Hook vào `connect`/`accept`: Giám sát kết nối mạng.
    * Hook vào `openat`: Giám sát truy cập file.
2.  **Prevention (Enforcement):**
    * Phát hiện hành vi bất thường (ví dụ: Container Web chạy lệnh `curl` tải malware hoặc sửa file `/etc/shadow`).
    * Thực hiện **Drop/Kill** process độc hại ngay lập tức từ tầng Kernel.
3.  **Anomaly Detection (Optional):** Tích hợp AI để học hành vi bình thường và cảnh báo bất thường.

### 🛠️ Framework & Tham khảo
* **Ngôn ngữ:** C (eBPF code), Go (User-space loader).
* **Tham khảo:** Falco, Tetragon, Cilium.

---

## 🗺️ Lộ trình học tập & Triển khai (Roadmap)

### Tháng 1-2: Nền tảng (Foundations)
- [ ] Nắm vững Docker: Viết `Dockerfile` tối ưu, `docker-compose.yml`.
- [ ] Kubernetes cơ bản: Hiểu sâu về Pod, Deployment, Service, Namespace, RBAC.
- [ ] **Thực hành:** Dựng cụm K8s Lab bằng Minikube trên Ubuntu/Linux.

### Tháng 3: Offensive Kubernetes (Red Teaming)
- [ ] Nghiên cứu về Kubernetes Security.
- [ ] Đọc sách: *"Hacking Kubernetes"* (Andrew Martin & Michael Hausenblas).
- [ ] **Thực hành:** Giải các bài Lab CTF trên **[Kubernetes Goat](https://github.com/madhuakula/kubernetes-goat)**.

### Tháng 4-5: Advanced & eBPF (Deep Tech)
- [ ] Tìm hiểu kiến trúc eBPF, Kernel Space vs User Space.
- [ ] Học cách sử dụng **Falco** (Runtime security tool).
- [ ] Cài đặt Falco vào K8s và kiểm tra log cảnh báo khi thực hiện tấn công.

---

## 💡 Tại sao chọn hướng đi này? (Motivation)

1.  **Cloud Native là tương lai:** Xu hướng chuyển dịch sang Microservices và K8s của các tập đoàn lớn (Banking, Fintech, E-commerce).
2.  **Thị trường ngách (Niche Market):** Nhân sự hiểu sâu về Linux Kernel và Container Security cực kỳ khan hiếm.
3.  **Thu nhập:** Cloud Security Engineer/DevSecOps là những vị trí có mức lương top đầu ngành bảo mật.

---

## 📚 Tài liệu tham khảo (Resources)

* [Kubernetes Goat - Vulnerable-by-design Cluster](https://github.com/madhuakula/kubernetes-goat)
* [eBPF.io - Introduction to eBPF](https://ebpf.io/)
* [Falco - Cloud Native Runtime Security](https://falco.org/)
* [OWASP Kubernetes Top 10](https://owasp.org/www-project-kubernetes-top-ten/)

---
*Created by [Your Name]*
