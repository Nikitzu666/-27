Выполнил: Смирнов Н.М.

Группа: ЭВТ-70

Игровой движок: Unity 2021.3.15

Название работы: разработка игры пинг-понг

Лабораторная работа №27

Тема: Разработка игрового проекта пинг-понг

Цель: приобрести навыки в разработке игрового проекта пинг-понг

Ход работы:

Выполнение работы

![image](https://user-images.githubusercontent.com/119733911/205580437-c3d6f50e-2de1-448d-9fdc-c72e58e4a1f9.png)

Рисунок 27.1. Файлы игры.

![image](https://user-images.githubusercontent.com/119733911/205580462-9e62691a-35f8-428b-a919-e97f2da19bbc.png)

Рисунок 27.2. Вид сцены.

![image](https://user-images.githubusercontent.com/119733911/205580476-a60c6203-0737-49f4-aeff-c8bb1a45d03c.png)

Рисунок 27.3. Вид игры.

![image](https://user-images.githubusercontent.com/119733911/205580486-aeaa0512-4337-4eb6-9159-34f11dc10afe.png)

Рисунок 27.4. Иерархия игры.

Листинг Ball.cs
public class Ball : MonoBehaviour
{
    public float speed = 300.0f;
    private Rigidbody2D _rigidbody;
    private void Awake()
    {
        _rigidbody = GetComponent<Rigidbody2D>();
    }	
    void Start()
    {
        ResetPosition();
        AddStartingForce();
    }
    public void ResetPosition()
    {
        _rigidbody.position = Vector3.zero;
        _rigidbody.velocity = Vector3.zero;
    }
    public void AddStartingForce()
    {
        float x = Random.value < 0.5f ? -0.1f : 1.0f;
        float y = Random.value < 0.5f ? Random.Range(-1.0f, -0.5f) :
                                        Random.Range(0.5f, 1.0f);
        Vector2 direction = new Vector2(x, y);
        _rigidbody.AddForce(direction * this.speed);
    }
    public void AddForce(Vector2 force)
    {
        _rigidbody.AddForce(force);
    }
}
Листинг BallSurface.cs
using UnityEngine;
public class BallSurface : MonoBehaviour
{
    public float bounceStrength;
    private void OnCollisionEnter2D(Collision2D collision)
    {
        Ball ball = collision.gameObject.GetComponent<Ball>();
        if (ball != null)
        {
            Vector2 normal = collision.GetContact(0).normal;
            ball.AddForce(-normal * this.bounceStrength);
        }
    }
}
Листинг ComputerPaddle.cs
public class ComputerPaddle : Paddle
{
    public Rigidbody2D ball;
    private void FixedUpdate()
    {
        if (this.ball.velocity.x > 0.0f)
        {
            if (this.ball.position.y > this.transform.position.y)
            {
                _rigidbody.AddForce(Vector2.up * this.speed);
            } 
            else if (this.ball.position.y < this.transform.position.y)
            {
                _rigidbody.AddForce(Vector2.down * this.speed);
            }
        }
        else
        {
            if (this.transform.position.y > 0.0f)
            {
                _rigidbody.AddForce(Vector2.down * this.speed);
            }
            else if (this.transform.position.y < 0.0f)
            {
                _rigidbody.AddForce(Vector2.up * this.speed);
            }
        }
    }
}
Листинг GameManager.cs
public class GameManager : MonoBehaviour
{
    public Ball ball;
    public Paddle computerPaddle;
    public Paddle playerPaddle;
    public TextMeshProUGUI PlayerScoreText;
    public TextMeshProUGUI computerScoreText;
    private int _playerScore;
    private int _computerScore;
    public void PlayerScores()
    {
        _playerScore++;
        this.PlayerScoreText.text = _playerScore.ToString();
        ResetRound();
       
    }
    public void ComputerScores()
    {
        _computerScore++;
        this.computerScoreText.text = _computerScore.ToString();
        ResetRound();
    }
    private void ResetRound()
    {
        this.computerPaddle.ResetPosition();
        this.playerPaddle.ResetPosition();
        this.ball.ResetPosition();
        this.ball.AddStartingForce();
    }
}

Листинг Paddle.cs
public class Paddle : MonoBehaviour
{
    public float speed = 10.0f;
    protected Rigidbody2D _rigidbody;
    private void Awake()
    {
        _rigidbody = GetComponent<Rigidbody2D>();
    }
    public void ResetPosition()
    {
        _rigidbody.position = new Vector2(_rigidbody.position.x, 0.0f);
        _rigidbody.velocity = Vector2.zero;
    }
}
Листинг PlayerPaddle.cs
public class PlayerPaddle : Paddle
{
    private Vector2 _direction;
    private void Update()
    {
        if (Input.GetKey(KeyCode.W) || Input.GetKey(KeyCode.UpArrow))
        {
            _direction = Vector2.up;
        }
        else if (Input.GetKey(KeyCode.S) || Input.GetKey(KeyCode.DownArrow))
        {
            _direction = Vector2.down;
        }
        else
        {
            _direction = Vector2.zero;
        }
    }
    private void FixedUpdate()
    {
        if (_direction.sqrMagnitude != 0)
        {
            _rigidbody.AddForce(_direction * this.speed);
        }
    }
}
Листинг ScoringZone.cs
public class ScoringZone : MonoBehaviour
{
    public EventTrigger.TriggerEvent scoreTrigger;
    private void OnCollisionEnter2D(Collision2D collision)
    {
        Ball ball = collision.gameObject.GetComponent<Ball>();
        if (ball != null)
        {
            BaseEventData eventData = new BaseEventData(EventSystem.current);
            this.scoreTrigger.Invoke(eventData);
        }
    }
}



Вывод
В ходе выполненной работы была разработана игра ping pong
