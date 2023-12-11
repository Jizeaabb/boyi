#基于博弈论的水下传感器网络短信号退避MAC协议所创建的简化理论模型
import tkinter as tk
import random

# 定义节点类
class Node:
    def __init__(self, node_id):
        self.node_id = node_id
        self.send_probability = 0
        self.backoff_probability = 0
        self.decision_log = []

    def set_send_probability(self, probability):
        self.send_probability = probability

    def set_backoff_probability(self, probability):
        self.backoff_probability = probability

    def send_or_wait(self):
        # 模拟节点发送或等待的决策
        decision = random.choices(['send', 'wait'], weights=[self.send_probability, 1 - self.send_probability])[0]
        self.decision_log.append(decision)
        return decision

# 模拟竞争过程
def simulate_competition(nodes):
    competition_log = []

    # 初始化节点的发送概率
    total_nodes = len(nodes)
    for node in nodes:
        node.set_send_probability(1 / total_nodes)

    sending_nodes = [node.node_id for node in nodes]
    competition_log.append(f"Competition Round 1 - All nodes compete to send")

    # 模拟竞争的多轮过程
    for i in range(2, 5):
        competition_log.append(f"\nCompetition Round {i}")
        for node in nodes:
            if node.node_id in sending_nodes:
                node.set_send_probability(0.8)  # 设置发送概率
            else:
                node.set_backoff_probability(0.6)  # 设置回退概率

            action = node.send_or_wait()
            if action == 'send':
                competition_log.append(f"Node {node.node_id} chooses to send")
            else:
                competition_log.append(f"Node {node.node_id} chooses to wait")

    return "\n".join(competition_log)

# 显示竞争结果的函数
def show_results():
    # 创建节点列表
    nodes_list = [Node(i) for i in range(1, 6)]
    # 模拟竞争并将结果展示在文本框中
    simulation_result = simulate_competition(nodes_list)
    result_text.delete('1.0', tk.END)
    result_text.insert(tk.END, simulation_result)

# 创建 UI
root = tk.Tk()
root.title("Competition Simulation")
root.geometry("400x300")

# 创建“Start Competition”按钮
start_button = tk.Button(root, text="Start Competition", command=show_results)
start_button.pack()

# 创建用于显示结果的文本框
result_text = tk.Text(root, height=20, width=40)
result_text.pack()

root.mainloop()
