# 5G-Core-Self-Healing
#  Cloud-Native 5G Core with eBPF & ML Self-Healing

مشروع تخرج لبناء شبكة جيل خامس (**5G Core**) سحابية فوق بيئة **Kubernetes**، مزودة بنظام مراقبة عميق للنواة باستخدام **eBPF**، ونظام تعافي ذاتي (**Self-Healing**) مدعوم بالذكاء الاصطناعي.

---

##  خطة العمل والجدول الزمني (Project Roadmap - 14 Weeks)

### المرحلة 1: الأساسيات وأدوات الفريق (الأسبوع 1 - 2)
- [ ] إتقان أوامر `Linux Terminal` وإدارة النظام والشبكات.
- [ ] تعلم `Git & GitHub` (الـ `Branches`, `Commits`, `Pull Requests`).
- [ ] تعلم كتابة ملفات `YAML` وتجهيز بيئة `VS Code`.
- [ ] **المخرج العملي (Deliverable):** رفع أول `Commit` من جميع أجهزة الفريق على هذا المستودع.

### المرحلة 2: البنية السحابية - Telco Cloud (الأسبوع 3 - 5)
- [ ] فهم وتطبيق أساسيات الحاويات باستخدام `Docker` و `Docker Compose`.
- [ ] دراسة معمارية `Kubernetes (K8s)` (الـ `Pods`, `Deployments`, `Services`).
- [ ] تثبيت بيئة `Kubernetes` محلية (`Minikube` أو `K3s`) على أجهزة الفريق.
- [ ] تعلم إدارة الحزم باستخدام `Helm Charts`.
- [ ] **المخرج العملي (Deliverable):** تشغيل `Kubernetes Cluster` محلي وإدارة الـ `Pods` بنجاح.

### المرحلة 3: بناء شبكة الجيل الخامس - 5G Core (الأسبوع 6 - 8)
- [ ] دراسة معمارية `5G SBA` وفهم دور (`AMF`, `SMF`, `UPF`, `NRF`).
- [ ] نشر شبكة `Open5GS` (أو `free5GC`) داخل `Kubernetes` باستخدام `Helm`.
- [ ] ربط محاكي الأبراج والموبايل `UERANSIM` بالشبكة.
- [ ] **المخرج العملي (Deliverable):** إجراء اتصال ناجح وسحب `Traffic` حقيقي عبر الـ `UPF`.

### المرحلة 4: المراقبة العميقة للنواة - eBPF & Observability (الأسبوع 9 - 11)
- [ ] دراسة أساسيات `eBPF` وكيفية مراقبة الـ `Packets` داخل الـ `Linux Kernel`.
- [ ] تشغيل أدوات سحب القراءات (`ebpf_exporter` / `BCC` / `Cilium Hubble`).
- [ ] ربط `Prometheus` ببيئة الكوبرنيتس لجمع الـ `Metrics` لحظياً.
- [ ] تصميم لوحة تحكم حية (`Dashboard`) على `Grafana`.
- [ ] **المخرج العملي (Deliverable):** عرض حالة شبكة الـ 5G ومعدل `Packet Drops` والـ `Latency` لايف على `Grafana`.

### المرحلة 5: الذكاء الاصطناعي والتعافي الذاتي - ML & Self-Healing (الأسبوع 12 - 14)
- [ ] سحب بيانات الشبكة من `Prometheus` وتجهيز الـ `Dataset` باستخدام `Pandas`.
- [ ] تدريب نموذج `Machine Learning` (`Anomaly Detection`) باستخدام `Scikit-Learn`.
- [ ] كتابة سكريبت الأتمتة بـ `Python` وربطه بـ `Kubernetes Python Client`.
- [ ] **المخرج العملي (Deliverable):** اكتشاف الأعطال أو الاختناق تلقائياً وعمل `Auto-Scaling` أو `Pod Restart` بدون تدخل بشري.

---

##  هيكلة ملفات المشروع (Repository Structure)
* `/docs`: التوثيق، الأبحاث، وملخصات الكورسات.
* `/k8s-5g-core`: ملفات الـ `YAML` والـ `Helm Charts` الخاصة بـ `Open5GS` و `UERANSIM`.
* `/ebpf-monitoring`: أكواد `eBPF` وإعدادات `Prometheus` و `Grafana`.
* `/ml-self-healing`: أكواد `Python`، نموذج الذكاء الاصطناعي، وسكريبت التحكم في `Kubernetes`.
