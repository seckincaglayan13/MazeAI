# MazeAI
yapay zeka destekli gelişigüzel bir labirent oyunu
#include <SFML/Graphics.hpp>
#include <iostream>
#include <stack>
#include <vector>
#include <ctime>
#include <cstdlib>

const int WINDOW_WIDTH = 640;
const int WINDOW_HEIGHT = 480;
const int CELL_SIZE = 20;
const int MAZE_WIDTH = WINDOW_WIDTH / CELL_SIZE;
const int MAZE_HEIGHT = WINDOW_HEIGHT / CELL_SIZE;

struct Cell {
    int row, col;
    bool visited;
    bool walls[4];
};

class Maze {
public:
    Maze(sf::RenderWindow& window) : m_window(window) {
        m_cells.resize(MAZE_WIDTH * MAZE_HEIGHT);
        for (int i = 0; i < MAZE_HEIGHT; ++i) {
            for (int j = 0; j < MAZE_WIDTH; ++j) {
                m_cells[i * MAZE_WIDTH + j].row = i;
                m_cells[i * MAZE_WIDTH + j].col = j;
                m_cells[i * MAZE_WIDTH + j].visited = false;
                m_cells[i * MAZE_WIDTH + j].walls[0] = true;
                m_cells[i * MAZE_WIDTH + j].walls[1] = true;
                m_cells[i * MAZE_WIDTH + j].walls[2] = true;
                m_cells[i * MAZE_WIDTH + j].walls[3] = true;
            }
        }

        srand(time(0));
        generateMaze();
    }

    void draw() {
        for (int i = 0; i < MAZE_HEIGHT; ++i) {
            for (int j = 0; j < MAZE_WIDTH; ++j) {
                if (m_cells[i * MAZE_WIDTH + j].walls[0])
                    m_window.draw(sf::RectangleShape(sf::Vector2f(CELL_SIZE, 1.f))).setPosition(sf::Vector2f(j * CELL_SIZE, i * CELL_SIZE));
                if (m_cells[i * MAZE_WIDTH + j].walls[1])
                    m_window.draw(sf::RectangleShape(sf::Vector2f(1.f, CELL_SIZE))).setPosition(sf::Vector2f(j * CELL_SIZE + CELL_SIZE, i * CELL_SIZE));
                if (m_cells[i * MAZE_WIDTH + j].walls[2])
                    m_window.draw(sf::RectangleShape(sf::Vector2f(CELL_SIZE, 1.f))).setPosition(sf::Vector2f(j * CELL_SIZE, i * CELL_SIZE + CELL_SIZE));
                if (m_cells[i * MAZE_WIDTH + j].walls[3])
                    m_window.draw(sf::RectangleShape(sf::Vector2f(1.f, CELL_SIZE))).setPosition(sf::Vector2f(j * CELL_SIZE, i * CELL_SIZE));
            }
        }
    }

    bool isWall(int row, int col, int direction) const {
        if (row < 0 || col < 0 || row >= MAZE_HEIGHT || col >= MAZE_WIDTH)
            return true;
        if (direction == 0)
            return m_cells[row * MAZE_WIDTH + col].walls[0];
        if (direction == 1)
            return m_cells[row * MAZE_WIDTH + col].walls[1];
        if (direction == 2)
            return m_cells[row * MAZE_WIDTH + col].walls[2];
        if (direction == 3)
            return m_cells[row * MAZE_WIDTH + col].
