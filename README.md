# gradient-descent-cpp

![Cpp11](https://img.shields.io/badge/C%2B%2B-11-blue.svg)
![License](https://img.shields.io/packagist/l/doctrine/orm.svg)
![Travis Status](https://travis-ci.org/Rookfighter/gradient-descent-cpp.svg?branch=master)
![Appveyor Status](https://ci.appveyor.com/api/projects/status/66uh2rua4sijj4y9?svg=true)

gradient-descent-cpp is a header-only C++ library for gradient descent
optimization using the ```Eigen3``` library.

## Install

Simply copy the header file into your project or install it using
the CMake build system by typing

```bash
cd path/to/repo
mkdir build
cd build
cmake ..
make install
```

The library requires ```Eigen3``` to be installed on your system.
In Debian based systems you can simply type

```bash
apt-get install libeigen3-dev
```

Make sure ```Eigen3``` can be found by your build system.

You can use the CMake Find module in ```cmake/``` to find the installed header.

## Usage

There are three steps to use gradient-descent-cpp:

* Implement your objective function as functor
* Instantiate the gradient descent optimizer
* Choose your parameters

```cpp
#include <gdcpp.h>

struct Ackley
{
    static double pi()
    { return 3.141592653589; }

    Ackley()
    { }

    double operator()(const Eigen::VectorXd &xval, Eigen::VectorXd &) const
    {
        assert(xval.size() == 2);
        double x = xval(0);
        double y = xval(1);
        // Calculate ackley function, but no gradient. Let gradien be estimated
        // numerically.
        return -20.0 * std::exp(-0.2 * std::sqrt(0.5 * (x * x + y * y))) -
            std::exp(0.5 * (std::cos(2 * pi() * x) + std::cos(2 * pi() * y))) +
            std::exp(1) + 20.0;
    }
};

int main()
{
    // Create optimizer object with Ackley functor as objective.
    //
    // You can specify a StepSize functor as template parameter.
    // There are ConstantStepSize,  BarzilaiBorwein and
    // WolfeBacktracking available. (Default is BarzilaiBorwein)
    //
    // You can additionally specify a Callback functor as template parameter.
    //
    // You can additionally specify a FiniteDifferences functor as template
    // parameter. There are Forward-, Backward- and CentralDifferences
    // available. (Default is CentralDifferences)
    gdc::GradientDescent<double, Ackley,
        gdc::WolfeBacktracking<double>> optimizer;

    // Set number of iterations as stop criterion.
    // Set it to 0 or negative for infinite iterations (default is 0).
    optimizer.setMaxIterations(200);

    // Set the minimum length of the gradient.
    // The optimizer stops minimizing if the gradient length falls below this
    // value (default is 1e-9).
    optimizer.setMinGradientLength(1e-6);

    // Set the minimum length of the step.
    // The optimizer stops minimizing if the step length falls below this
    // value (default is 1e-9).
    optimizer.setMinStepLength(1e-6);

    // Set the momentum rate used for the step calculation (default is 0.0).
    // Defines how much momentum is kept from previous iterations.
    optimizer.setMomentum(0.4);

    // Turn verbosity on, so the optimizer prints status updates after each
    // iteration.
    optimizer.setVerbosity(4);

    // Set initial guess.
    Eigen::VectorXd initialGuess(2);
    initialGuess << -2.7, 2.2;

    // Start the optimization
    auto result = optimizer.minimize(initialGuess);

    std::cout << "Done! Converged: " << (result.converged ? "true" : "false")
        << " Iterations: " << result.iterations << std::endl;

    // do something with final function value
    std::cout << "Final fval: " << result.fval << std::endl;

    // do something with final x-value
    std::cout << "Final xval: " << result.xval.transpose() << std::endl;

    return 0;
}
```
.
<<<<<<< HEAD
.
=======
change here
haha, create conflict
>>>>>>> be73c290106b4aba74dbd2316f4be800fa01d047
