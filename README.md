  <h1>Angular Or Filter</h1>
    <article>
        Full text search does not supported
    </article>
    <h2>Demo</h2>
    <h2>Filter Of Object List</h2>
    <code>[{...}, {...}, {...} ...]</code>
    <code>$filter('orFilter')(scope.collections.friends, {arg1, arg2, ...}, false)</code>
    <pre class="highlight highlight-html">
    <label for="name">Get Name</label>
    <select id="name" ng-options="item.name for item in collections.friends" ng-model="selectedItem" ng-change="filtered()"></select>
    <input id="male" type="checkbox" ng-model="male" ng-change="filtered()" />
    <label for="male">Or Get Male</label>
    <ul>
    <li ng-repeat="item in filteredList">
            {{ item.name }} - {{ (item.gender ? 'Male' : 'Female') }}
        </li>
    </ul>
    <script>
    ; (function (app) {
        app.controller("someCtrl", [
            "$scope", "$filter", function (scope, filter) {
                scope.filteredList = [];

                scope.filtered = function () {
                    var expression = {};
                    if (scope.male) expression.gender = scope.male;
                    if (scope.selectedItem) expression.name = scope.selectedItem.name;

                    scope.filteredList = filter('orFilter')(scope.collections.friends, expression, false);
                };

                scope.pageLoad = function () {
                    scope.filteredList = scope.collections.friends;
                }.call(this);
            }
        ]);

        app.run([
            "$rootScope", function (root) {
                root.collections = {};

                root.collections.friends = [
                    { name: "Anna Marquez", gender: false },
                    { name: "Veronica Morton", gender: false },
                    { name: "Vinson Glover", gender: true },
                    { name: "Parker Howard", gender: true }
                ];

                root.collections.numbers = [1, 2, 3, 4, 5, "5", "4", "3"];

                root.collections.strings = ["Craig", "Trisha", "Adeline", "Elvia", "Knight"];
            }
        ]);
    })(angular.module('app', ['or-filter']));
    </script>
    </pre>
    <h2>Filter of Number in Javascript</h2>
    <pre class="highlight highlight-html">
    <script>
    var list = [1, 2, 3, 4, 5, "5", "4", "3"];
    $scope.filteredList = filter('orFilter')(scope.collections.friends, [1, 3, 4], false); // return [1, 3, 4 "4", "3"]
    $scope.filteredList = filter('orFilter')(scope.collections.friends, [1, 3, 4], true); // return [1, 3, 4]
    $scope.filteredList = filter('orFilter')(scope.collections.friends, 1, true); // return [1]
    </script>
    </pre>

    <h2>Filter of Number in HTML</h2>
    <pre class="highlight highlight-html">
    <ul>
    <li ng-repeat="item in [1, 2, 3, 4, 5, '5', '4', '3'] | orFilter: 5"> {{ item }} </li>
        <!--
            Result:
            <li>5</li>
            <li>5</li>
        -->
    <li ng-repeat="item in [1, 2, 3, 4, 5, '5', '4', '3'] | orFilter: 5: true"> {{ item }} </li>
        <!--
            Result:
            <li>5</li>
        -->
    <li ng-repeat="item in [1, 2, 3, 4, 5, '5', '4', '3'] | orFilter: [1, 4]"> {{ item }} </li>
        <!--
            Result:
            <li>1</li>
            <li>4</li>
            <li>4</li>
        -->
    </ul>
    </pre>
    <h2>Filter of String in Javascript</h2>
    <pre class="highlight highlight-html">
    <script>
    var list = ["Craig", "Trisha", "Adeline", "Elvia", "Knight"];
    $scope.filteredList = filter('orFilter')(scope.collections.friends, ["Craig", "Trisha"]); // return ["Craig", "Trisha"]
    $scope.filteredList = filter('orFilter')(scope.collections.friends, "Craig"); // return ["Craig"]
    </script>
    </pre>

    <h2>Filter of String in HTML</h2>
    <pre class="highlight highlight-html">
    <ul>
    <li ng-repeat="item in ['Craig', 'Trisha', 'Adeline', 'Elvia', 'Knight'] | orFilter: 'Craig'"> {{ item }} </li>
        <!--
            Result:
            <li>Craig</li>
            -->
    <li ng-repeat="item in ['Craig', 'Trisha', 'Adeline', 'Elvia', 'Knight'] | orFilter: 'craig': true"> {{ item }} </li>
        <!--
            Result:
            -- empty --
            -->
    <li ng-repeat="item in ['Craig', 'Trisha', 'Adeline', 'Elvia', 'Knight'] | orFilter: ['Craig', 'Elvia']"> {{ item }} </li>
        <!--
            Result:
            <li>Craig</li>
            <li>Elvia</li>
            -->
    </ul>
</pre>
