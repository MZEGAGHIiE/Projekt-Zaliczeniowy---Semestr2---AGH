# Prosta implementacja Langton's Ant

    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib.animation as animation

    class LangtonsAnt:
    def __init__(self, grid_size=100, steps=11000):
        self.grid_size = grid_size
        self.steps = steps
        # Zainicjuj siatkę: 0 = biały, 1 = czarny
        self.grid = np.zeros((grid_size, grid_size), dtype=int)
        # Rozpocznij z mrówką na środku
        self.x, self.y = grid_size // 2, grid_size // 2
        # Kierunki: 0=góra, 1=prawo, 2=dół, 3=lewo
        self.direction = 0

    def step(self):
        # Zmień kolor
        self.grid[self.x, self.y] ^= 1
        # Skręt: biały->prawo, czarny->lewo
        if self.grid[self.x, self.y] == 0:
            self.direction = (self.direction + 1) % 4
        else:
            self.direction = (self.direction - 1) % 4
        # Przesuń się do przodu
        if self.direction == 0:
            self.x -= 1
        elif self.direction == 1:
            self.y += 1
        elif self.direction == 2:
            self.x += 1
        elif self.direction == 3:
            self.y -= 1
        # Zwrot na krawędziach
        self.x %= self.grid_size
        self.y %= self.grid_size

    def run(self):
        for _ in range(self.steps):
            self.step()
        return self.grid

    def animate(self, interval=1, save=False, filename='langtons_ant.gif'):
        fig, ax = plt.subplots()
        img = ax.imshow(self.grid, cmap='binary')
        ax.set_xticks([])
        ax.set_yticks([])

        def update(frame):
            self.step()
            img.set_data(self.grid)
            return (img,)

        ani = animation.FuncAnimation(fig, update, frames=self.steps,
                                      interval=interval, blit=True, repeat=False)
        if save:
            ani.save(filename, writer='imagemagick')
        else:
            plt.show()

    if __name__ == '__main__':
    ant = LangtonsAnt(grid_size=200, steps=11000)
    # Aby tylko uruchomić i otrzymać na końcu siatkę:
    final = ant.run()
    plt.figure(figsize=(6,6))
    plt.imshow(final, cmap='binary')
    plt.title("Langton's Ant - Stan końcowy")
    plt.axis('off')
    plt.show()
    
