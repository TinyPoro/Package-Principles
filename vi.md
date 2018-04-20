## Các nguyên tắc trong thiết kế package
Ngày hôm qua, tôi đã được yêu cầu nghiên cứu cuốn Các nguyên tắc của thiết kế package, cái mà Robert Martin đã chỉ ra trong cuốn Các nguyên tắc, mẫu và thực hành phát triển phần mềm theo Agile.
Tôi đã gặp 1 chút khó khăn. Tôi nghĩ là do 2 lí do. Đầu tiên, tôi chưa từng làm việc trên các dự án lớn, nhiều người mà liên quan nhiều package, vì vậy tôi không cảm nhận trực tiếp được khó khăn được mô tả. Vì vậy tôi phải cố gắng tưởng tượng các kịch bản thay vì có thể liên tưởng đến các ví dụ cụ thể, cá nhân hơn.Thứ hai, tôi thấy rằng sẽ dễ dàng hơn khi học bằng cách bắt đầu với vài thứ rõ ràng và sau đó mở rộng nó ra thành các nguyên tắc chung, hơn là việc
I found this a bit of a struggle. I think, for two reasons. First, I have not worked on a large, multi-person project involving multiple packages, so have not felt the pain described directly. So I have to try to imagine the scenario rather than being able to relate the narrative to some concrete, personal examples. Second, I find it easier to learn by starting with something concrete and then expanding that to an abstract principle, rather than applying some abstract principle as I go. Which is harder with something like package design principles. All that being said, here is what I took away from it.

#### Bottom up design
First, beyond the principles themselves, there is a general concept that package design should be “bottom-up”, which means it is likely to reveal itself as classes are created and their interrelationship becomes clear.

An outcome of this bottom-up approach is that the package structure will evolve over time, as relationships become clearer and a project develops: what Martin describes as “gitter”.

This, of course, makes sense in the context of TDD and the SOLID design principles, where code is constantly open to improvement and refactoring. If this applies to methods and classes, it follows that it should also apply to the way those methods and classes are packaged together.

#### Logical structures vs “developability”
Martin identifies that this bottom-up approach can however create a tension between creating packages which follow the logical structures of the application’s domain and creating packages that aid code reuse and “developability”. Martin’s principles tend to favour developability over logical structures.

#### Granularity: The Principles of Package Cohesion
The first three principles concern how to partition classes into packages.

(1) The Reuse-Release Equivalence Principle (REP)
A package maintainer must think of their obligations to the package users. These obligations should be clear from the outset. This means adopting standards like Semver for releases, as well as being clear about the nature and extent of a maintainer’s obligations for older versions.

(2) The Common-Reuse Principle (CRP)
When we update a class in a package, we have to republish the entire package. But we do not want a user to have to update to a new package because of changes to classes on which they do not depend. It therefore makes sense to group classes into packages, so that the package user is likely to use all the classes in that package.

(3) The Common-Closure Principle (CCP)
Packages should favour maintainability over reusability. It is therefore helpful to limit the reasons for a package to change. This can be achieved at the package level by grouping together classes which are likely to change for the same reasons.

#### Stability: The Principles of Package Coupling
The final three principles concern how to structure multiple packages.

(4) The Acyclic Dependencies Principle (ADP)
This seems to me to be the most important principle for building maintainable packages. Cyclic dependencies are a bad idea. First, whether between classes or packages, cyclic dependencies create complex interactions and therefore it is hard to predict the impact of those changes. Second, suddenly a change to any package in an application, triggers an update in every package. One of the main benefits of packages is to create clear barriers between packages, allowing different packages to develop at different paces and allowing developers to work on smaller parts of a system without needing to fully understand the whole. These benefits are lost as soon as there are acyclic dependencies.

Dependency Inversion
One design pattern Martin identifies is the Dependency Inversion Principle. This is best illustrated with a simple example. If we have a Package A, with a Class A, which references a Class B in Package B, so that Package A depends on Package B. We can invert this dependency by making a new Interface A in Package A, and make Class A use Interface A instead of Class B. We can then make Class B implement Interface A. Now Package B depends on Package A (see the Wikipedia article in the link above).

(5) The Stable Dependencies Principle (SDP)
Applications are subject to change and so some packages in an application will change. Different packages will have different levels of stability. The aim of this principle is to ensure that stable packages don’t rely on unstable packages. This makes sense, because if Package A relies on Package B, then a change in Package B may trigger a need for a change in Package A. It therefore makes sense that we would want Package B to be less susceptible to change than Package A.

(6) The Stable-Abstractions Principle (SAP)
Following on from the SDP, this principle says that, in order for a package to be stable, it needs to be less susceptible to change, and so should be more abstract. Abstract classes are designed to be sub-classed, so there features can be extended without need for modification. This makes them more stable. Packages with many dependents should contain more abstract classes. Whereas packages with few dependents should be contain more concrete classes.