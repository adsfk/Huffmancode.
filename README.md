class HuffmanNode:
    def __init__(self, frequency, symbol=None, left=None, right=None):
        self.frequency = frequency
        self.symbol = symbol
        self.left = left
        self.right = right

    # Helper method to check if a node is a leaf (has no children)


class HuffmanCoding:
    def __init__(self, frequencies): # frequencies -> dict, with char: freq
        # Initialize with a list of Huffman nodes for each character
        self.nodes = [HuffmanNode(freq, symbol) for symbol, freq in frequencies.items()]
        self.codes = {}
      
    def small_nodes(self):
        self.nodes=sorted(self.nodes, key=lambda node: node.frequency)
        return [self.nodes[0], self.nodes[1]]

    def is_leaf(self, node):
        return node.left is None and node.right is None

    def get_codes(self):
        while len(self.nodes)>1:
          l, r = self.small_nodes()
          self.nodes.remove(l)
          self.nodes.remove(r)
          new_node= HuffmanNode(l.frequency+r.frequency, None, l, r)
          self.nodes.append(new_node)
        root = self.nodes[0]
        self.generate_codes(root, "")
        return self.codes

    def generate_codes(self, node=None, current_code=""):

        if node is None:
            node = self.nodes[0]

        if self.is_leaf(node): 
            self.codes[node.symbol] = current_code 
            return
      
        self.generate_codes(node.left, current_code + "0") # 왼쪽 노드로 돌아가면서 0 추가
        self.generate_codes(node.right, current_code + "1") # 오른쪽 노드로 돌아가면서 1 추가

# Example usage
if __name__ == "__main__":
    # Input frequencies as per Table 9.1.2
    frequencies = {
        '!': 2,
        '@': 3,
        '#': 7,
        '$': 8,
        '%': 12
    }

    # Create HuffmanCoding instance and get the codes
    huffman_coding = HuffmanCoding(frequencies)
    codes = huffman_coding.get_codes()

    # Output the resulting Huffman codes
    print("Huffman Codes for the given frequencies:")
    for symbol, code in codes.items():
        print(f"{symbol}: {code}")
