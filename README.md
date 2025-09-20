# 基于扩散模型的生成式飞行器外观设计



## 相关数据集



## 1. Generative Aerodynamic Design with Diffusion Probabilistic Models (Airbus)

- **数据集**
  随机采样 Bernstein 多项式参数化翼型，并用 XFOIL 计算气动特性。

- **规模**  
  共 1000 个翼型：训练集 600，验证集 200，测试集 200。

- **几何表示**  
  - 上下翼面均采用 6 阶 Bernstein 多项式；  
  - 共 11 个参数（因首项约束，报告中有解释）。

- **参数**  
  - 升力系数 **CL**  
  - 阻力系数 **CD**  
  - 力矩系数 **CM**  
  - 仿真条件：Re = 1×10⁶，攻角 α = 5°。

- **数据处理**  
  - 去除自交和非物理翼型；  
  - 用 Mahalanobis distance 去掉 99.5% 以上离群点。  



## 2. DiffAirfoil: Latent Space Diffusion Model

- **数据集**  
  UIUC Airfoil Database。

- **规模**  
  构建多个子集：1000 / 500 / 250 / 100 / 50 个翼型。

- **几何表示**  
  - 使用 Latent Space Model (LSM) 将翼型轮廓点集映射到 256 维潜空间；  
  - 训练时采用 Chamfer Distance 作为几何重建损失。

- **参数**  
  - 可施加条件：  
    - 翼型面积 (A)  
    - 最大厚度 (MT)  
    - 弯度位置 (C2C) 等。

- **特点**  
  - 数据效率高，仅需 50 个翼型也能训练；  
  - 支持条件生成，生成结果有效且平滑。  



## 3. Compositional Generative Inverse Design

### 数据集 1：N-body Interaction (动力学)
- **来源**：用 Pymunk + Pygame 仿真 N 个球体在盒子内弹性碰撞。 【不相关】 

### 数据集 2：2D Airfoil Design (Navier-Stokes)
- **来源**：LilyPad 流体仿真器。  
- **空间分辨率**：64×64 网格。  
- **流场变量**：每个格点包含 (vx, vy, p)。  
- **边界表示**：64×64×3 张量：  
  - mask（二值，固体/流体）、  
  - 最近边界点相对位置 (Δx, Δy)。  
- **维度**：边界参数高达 12288。  
- **设计目标**：最小化 (-Lift + Drag)，最大化升阻比 L/D。  

