# **Section 2 – Kubernetes Scaling **

> **Note:** Diagrams cannot be embedded as images inside the canvas editor.
> I have converted your notebook diagrams into clean **ASCII diagrams** so your GitHub README still looks professional.

---

# ## **1. Kubernetes Scaling Overview**

Kubernetes provides multiple layers of scaling:

* **HPA (Horizontal Pod Autoscaler)** → scales *pods*.
* **Cluster Autoscaler** → scales *nodes*.
* **VPA (Vertical Pod Autoscaler)** → scales *pod resources*.
* **Karpenter** → advanced autoscaling & node provisioning.

---

# ## **2. Horizontal Pod Autoscaler (HPA)**

HPA increases/decreases pod replicas based on metrics.

### **How HPA Works**

* If CPU/Memory usage increases → add pods.
* If load drops → remove pods.

### **ASCII Diagram – HPA Scaling**

```
Traffic ↑
        +-----------+
        |   HPA     |
        +-----------+
          |   |   |
     +----+   |   +----+
     |        |        |
  +------+ +------+ +------+
  | Pod1 | | Pod2 | | Pod3 |
  +------+ +------+ +------+
```

---

# ## **3. Cluster Autoscaler (Node Autoscaler)**

Cluster Autoscaler adds/removes EC2 nodes when existing nodes run out of pod capacity.

### **Example**

When nodes hit max pods → Kubernetes cannot schedule → Cluster Autoscaler provisions new nodes.

### **ASCII Diagram – Node Autoscaling**

```
Node-1 (Full)      Node-2 (Full)     Node-3 (Needs more pods)
[ P P P ]          [ P P P ]         [ P P _ ]
                                      |
                                      v
                              New Node Created
```

---

# ## **4. Pod Requests & Limits**

These define **minimum guaranteed resources** and **maximum allowed resources**.

### **CPU Conversion**

```
1 CPU = 1000m
0.5 CPU = 500m
```

### **Example Pod**

```
requests:
  cpu: 500m
  memory: 256Mi
limits:
  cpu: 1000m
  memory: 512Mi
```

### **ASCII Diagram – Requests vs Limits**

```
CPU Request (0.5 CPU) ---------
CPU Limit   (1 CPU)       --------------
```

---

# ## **5. Vertical Pod Autoscaler (VPA)**

VPA changes **pod size (CPU/Memory)** dynamically.

⚠ **Should NOT be used in production with HPA**
Reason: VPA *restarts pods* when applying changes.

### **Modes**

```
updateMode: Off   → Only gives recommendations
updateMode: Auto  → Applies changes (restarts pods)
```

### **ASCII Diagram – VPA Behavior**

```
Pod Usage ↑ → VPA Suggests:
CPU from 200m → 400m
Memory from 256Mi → 512Mi
```

---

# ## **6. Goldilocks (VPA Helper Tool)**

Goldilocks shows actual recommended CPU/Memory for each deployment.

### What it does:

* Shows **VPA recommendations** per namespace.
* Helps tune Requests & Limits.

---

# ## **7. Karpenter Autoscaling**

Karpenter is a modern autoscaler that provisions **right-sized EC2 nodes** instantly.

### **Problem with Default Cluster Autoscaler**

If you need 3 nodes for 9 pods:

```
Node1: 3 pods
Node2: 3 pods
Node3: 1 pod  → Wasted space
```

* Slow provisioning
* Inefficient packing

### **Karpenter Solution**

Karpenter provisions *optimal EC2 instance sizes* based on pod requirements.

### **ASCII Diagram – Efficient Packing by Karpenter**

```
Instead of 3 × t3.medium nodes:

EC2: m5.2xlarge
[ P P P P P P P P P ]  → All pods fit perfectly
```

### **Benefits**

* No need for nodegroups
* Automatically picks best EC2 type
* Faster provisioning
* Supports GPU pods without GPU nodegroup creation

---

# **End of Section 2 **
