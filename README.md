<a href="https://twitter.com/home"><img src="https://cdn.discordapp.com/attachments/1125487567257739356/1125950651155881984/1200px-Laravel.svg_preview_rev_1.png"></a>

<h1>API em <a href="https://laravel.com/"><img src="https://laravel.com/img/logotype.min.svg"></a> - Conclusão da matéria Criação de API Rest básica com PHP </h1>

Acesse o repositorio do trabalho e clone ele pelo git bash para ter a base solicitada pelo professor
<details>
  <summary>✍️ Link do repositório da tarefa</summary>
  <br/>

  **https://github.com/joaovcandrade/projeto-laravel**

  </div>
</details> 

1- Acessar a pasta app/Http/Controllers/TaskController.php e trocar pelo seguinte codigo:
 
 ```
 <?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Task;

class TaskController extends Controller
{
    public function index()
    {
        $tasks = Task::all();

        return response()->json($tasks);
    }

    public function show($id)
    {
        $task = Task::find($id);

        if ($task) {
            return response()->json($task);
        } else {
            return response()->json(['error' => 'Tarefa não encontrada'], 404);
        }
    }

    public function store(Request $request)
    {
        $request->validate([
            'title' => 'required',
            'description' => 'required',
        ]);

        $task = new Task;
        $task->title = $request->input('title');
        $task->description = $request->input('description');
        $task->status = $request->input('status', false);
        $task->save();

        return response()->json($task, 201);
    }

    public function update(Request $request, $id)
    {
        $task = Task::find($id);

        if ($task) {
            $request->validate([
                'title' => 'required',
                'description' => 'required',
            ]);

            $task->title = $request->input('title');
            $task->description = $request->input('description');
            $task->status = $request->input('status', $task->status);
            $task->save();

            return response()->json($task);
        } else {
            return response()->json(['error' => 'Tarefa não encontrada'], 404);
        }
    }

    public function destroy($id)
    {
        $task = Task::find($id);

        if ($task) {
            $task->delete();

            return response()->json(['message' => 'Tarefa excluída com sucesso']);
        } else {
            return response()->json(['error' => 'Tarefa não encontrada'], 404);
        }
    }
}
 ```
2- Acessar a pasta database/migrations/2023_06_27_221132_create_tasks_table.php e colocar o seguinte codigo:
  
 
 ```
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateTasksTable extends Migration
{
    public function up()
    {
        Schema::create('tasks', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('description');
            $table->boolean('status')->default(false);
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('tasks');
    }
}
 ```

3- Acessar a pasta routes/api.php e colocar o seguinte codigo:
 
 ```
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/tasks', 'TaskController@index');
Route::get('/tasks/{id}', 'TaskController@show');
Route::post('/tasks', 'TaskController@store');
Route::put('/tasks/{id}', 'TaskController@update');
Route::delete('/tasks/{id}', 'TaskController@destroy');
 ```
4- Acessar a pasta routes/web.php e colocar o seguinte codigo:
 
 ```
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\TaskController;

Route::get('/tasks', [TaskController::class, 'index']);
Route::get('/tasks/{id}', [TaskController::class, 'show']);
Route::post('/tasks', [TaskController::class, 'store']);
Route::put('/tasks/{id}', [TaskController::class, 'update']);
Route::delete('/tasks/{id}', [TaskController::class, 'destroy']);



 ```
Apos isso abra o terminar e insira o comando "composer install" para instalar as dependências do projeto

Inicia no XAMPP o Apache e o MYSQL

E execute no terminal o comando "php artisan serve" para inciciar o servidor

<h2>Link para o vídeo testando a API no Postman</h2>
<a href="https://www.youtube.com/watch?v=R_hAjIFm8Bg"><img src="https://cdn.discordapp.com/attachments/668195190829219887/1125960945223602256/youtube-logo.png" width=550 height=350></a>

